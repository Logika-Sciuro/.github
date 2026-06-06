# Shared CI/CD Pipelines Strategy

## Overview

This document outlines the strategy for implementing shared, reusable CI/CD pipelines across the Logika-Sciuro organization to standardize build, test, and validation checks while minimizing duplication and reducing time-to-CI for new repositories.

## Current State

### Existing Infrastructure
- ✅ Centralized label management (`.github/workflows/sync-labels.yml`)
- ✅ Sprint/milestone synchronization (`.github/workflows/sync-sprints-milestones.yml`)
- ✅ Organizational PR template and commit instructions
- ✅ Pre-commit hooks in `cv-engine` and `docs` (local only)

### CI/CD Gaps
| Repository | Build | Test | Lint | Status |
|---|---|---|---|---|
| scavenger-core-gateway | ❌ | ❌ | ❌ | Java 21 + Maven |
| scavenger-cv-engine | ❌ | ❌ | ⚠️ | Python 3.11+ (pre-commit only) |
| scavenger-e2e-framework | ❌ | ❌ | ❌ | Java (framework TBD) |
| scavenger-infra-as-code | ❌ | ❌ | ❌ | Terraform + K8s |
| docs | ⚠️ | ❌ | ✅ | MkDocs + commitlint |

---

## Strategy: Reusable Workflows

### Design Principles

1. **Reusability**: Workflows in `.github/.github/workflows/` can be called from any repository
2. **Consistency**: Standardized job names and check names for branch protection rules
3. **Flexibility**: Inputs allow customization (versions, directories, check names)
4. **Minimalism**: No secrets embedded; use organization secrets or OIDC where needed
5. **Incremental**: Start with foundational builds; extend later with security scanning

### Workflow Structure

```
.github/.github/workflows/
├── java-build.yml           # Reusable Java/Maven pipeline
├── python-build.yml         # Reusable Python/pip pipeline
├── terraform-validate.yml   # Reusable Terraform validation
├── mkdocs-build.yml         # Reusable MkDocs documentation build
└── README.md                # Documentation on usage
```

### Shared Workflow Specifications

#### 1. Java Build Workflow (`java-build.yml`)

**Purpose**: Build, test, and verify Java projects with Maven

**Inputs**:
- `java-version` (default: `21`)
- `maven-version` (default: `3.9.x`)
- `check-name` (default: `build`)

**Jobs**:
1. Compile: `mvn clean package -DskipTests`
2. Test: `mvn test`
3. Quality: `mvn verify`

**Called by**:
- `scavenger-core-gateway` on PR + merge to main
- `scavenger-e2e-framework` on PR + merge to main

**Example consumer workflow**:
```yaml
name: Build

on: [pull_request, push]

jobs:
  build:
    uses: logika-sciuro/.github/.github/workflows/java-build.yml@main
    with:
      java-version: '21'
      check-name: 'build'
```

---

#### 2. Python Build Workflow (`python-build.yml`)

**Purpose**: Lint, test, and validate Python projects

**Inputs**:
- `python-version` (default: `3.11`)
- `check-name` (default: `build`)

**Jobs**:
1. Setup: Python env + pip dependencies
2. Lint: flake8 with configurable rules
3. Test: pytest (if available)

**Called by**:
- `scavenger-cv-engine` on PR + merge to main

**Example consumer workflow**:
```yaml
name: Build

on: [pull_request, push]

jobs:
  build:
    uses: logika-sciuro/.github/.github/workflows/python-build.yml@main
    with:
      python-version: '3.11'
      check-name: 'build'
```

---

#### 3. Terraform Validation Workflow (`terraform-validate.yml`)

**Purpose**: Validate Terraform configuration and check formatting

**Inputs**:
- `terraform-version` (default: `1.8`)
- `working-directory` (default: `.`)
- `check-name` (default: `terraform`)

**Jobs**:
1. Format Check: `terraform fmt -check -recursive`
2. Init: `terraform init -backend=false`
3. Validate: `terraform validate`

**Called by**:
- `scavenger-infra-as-code` on PR + merge to main

**Example consumer workflow**:
```yaml
name: IaC Validation

on: [pull_request, push]

jobs:
  terraform:
    uses: logika-sciuro/.github/.github/workflows/terraform-validate.yml@main
    with:
      terraform-version: '1.8'
      working-directory: './terraform'
      check-name: 'terraform'
```

---

#### 4. MkDocs Build Workflow (`mkdocs-build.yml`)

**Purpose**: Build and validate MkDocs documentation

**Inputs**:
- `node-version` (default: `20`)
- `check-name` (default: `docs`)

**Jobs**:
1. Setup: Node.js + npm dependencies
2. Build: `mkdocs build --strict`
3. Verify: Check `site/` directory exists

**Called by**:
- `docs` on PR + merge to main

**Example consumer workflow**:
```yaml
name: Docs

on: [pull_request, push]

jobs:
  docs:
    uses: logika-sciuro/.github/.github/workflows/mkdocs-build.yml@main
    with:
      node-version: '20'
      check-name: 'docs'
```

---

## Adoption Plan

### Phase 1: Foundation (Current)
- ✅ Create reusable workflows in `.github/.github/workflows/`
- ✅ Document usage and examples
- ⏳ **Next**: Create caller workflows in each target repo

### Phase 2: Adoption (Week 1-2)
1. Add `.github/workflows/build.yml` to each repo
   - `scavenger-core-gateway`: use `java-build.yml`
   - `scavenger-cv-engine`: use `python-build.yml`
   - `scavenger-e2e-framework`: use `java-build.yml`
   - `scavenger-infra-as-code`: use `terraform-validate.yml`
   - `docs`: use `mkdocs-build.yml`

2. Test locally before enforcing branch protection
3. Monitor first 5-10 runs for issues

### Phase 3: Protection (Week 2+)
- Enable branch protection rules requiring checks to pass
- Standardized check names:
  - `build` (Java/Python/Terraform/Docs)
  - Additional checks: `commitlint` (existing)

### Phase 4: Future Extensions
- Add security scanning (SonarQube, CodeQL, dependency checks)
- Add container image builds
- Add deployment workflows

---

## Branch Protection Rules

Once adopted, enforce these checks on `main`:

```
Required status checks:
✓ build (all repos)
✓ commitlint (all repos, already configured in docs)
✓ codeql (when security scanning is added)
```

---

## Implementation Checklist

### .github Repository
- [ ] `java-build.yml` created ✅
- [ ] `python-build.yml` created ✅
- [ ] `terraform-validate.yml` created ✅
- [ ] `mkdocs-build.yml` created ✅
- [ ] `SHARED_WORKFLOWS.md` created (this doc) ✅
- [ ] Update root `README.md` to reference shared workflows

### Consumer Repositories (per-repo in separate PRs)
- [ ] `scavenger-core-gateway`: Add caller workflow
- [ ] `scavenger-cv-engine`: Add caller workflow
- [ ] `scavenger-e2e-framework`: Add caller workflow
- [ ] `scavenger-infra-as-code`: Add caller workflow
- [ ] `docs`: Update caller workflow

---

## Naming Conventions

### Job Names (visible in UI)
- `build` - Generic for all toolchains
- `docs` - Documentation specific
- `terraform` - IaC specific

### Check Names (for branch protection)
Standardized across org:
- `build` - Status check exposed to branch protection
- `commitlint` - Already configured

### Workflow Files
- Pattern: `<tool>-<action>.yml` (e.g., `java-build.yml`)
- Reusable workflows stored in `.github/.github/workflows/`
- Consumer workflows stored in `<repo>/.github/workflows/`

---

## Future Improvements

1. **Composite Actions**: Consider composite actions for multi-step workflows
   - `.github/.github/actions/setup-java/action.yml`
   - `.github/.github/actions/setup-python/action.yml`

2. **Build Matrix**: Support multiple Java/Python versions
   - `strategy.matrix.java-version: [17, 21]`

3. **Artifact Management**: Store build artifacts for later stages
   - Upload to GHA artifacts store or package registries

4. **Deployment**: Chain workflows for deploy-on-merge
   - Call `deploy.yml` after successful builds

5. **Security**: Add SAST, DAST, and supply chain security
   - CodeQL scanning
   - Dependabot alerts
   - SBOM generation

---

## References

- GitHub Actions: https://docs.github.com/en/actions/using-workflows/reusing-workflows
- Composite Actions: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action
- Maven: https://maven.apache.org/
- Python/pytest: https://pytest.org/
- Terraform: https://www.terraform.io/docs/cli/
- MkDocs: https://www.mkdocs.org/

---

## Questions & Support

For issues or questions about shared workflows:
1. Check this document and workflow examples
2. File an issue in `.github` repository with label `ci`
3. Contact: Engineering team
