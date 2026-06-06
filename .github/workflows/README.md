# Shared Workflows

This directory contains reusable GitHub Actions workflows that can be called from repositories across the Logika-Sciuro organization.

## Available Workflows

### Java Build (`java-build.yml`)
Builds, tests, and validates Java projects using Maven.

**Inputs**:
```yaml
java-version:      '21'      # Java version to use
maven-version:     '3.9.x'   # Maven version
check-name:        'build'   # Name of the check (for branch protection)
```

**Usage**:
```yaml
jobs:
  build:
    uses: logika-sciuro/.github/.github/workflows/java-build.yml@main
    with:
      java-version: '21'
      check-name: 'build'
```

---

### Python Build (`python-build.yml`)
Lints, tests, and validates Python projects using flake8 and pytest.

**Inputs**:
```yaml
python-version:    '3.11'    # Python version to use
check-name:        'build'   # Name of the check
```

**Usage**:
```yaml
jobs:
  build:
    uses: logika-sciuro/.github/.github/workflows/python-build.yml@main
    with:
      python-version: '3.11'
      check-name: 'build'
```

---

### Terraform Validate (`terraform-validate.yml`)
Validates Terraform configuration and checks formatting.

**Inputs**:
```yaml
terraform-version:       '1.8'      # Terraform version
working-directory:       '.'        # Directory containing terraform code
check-name:              'terraform' # Name of the check
```

**Usage**:
```yaml
jobs:
  terraform:
    uses: logika-sciuro/.github/.github/workflows/terraform-validate.yml@main
    with:
      terraform-version: '1.8'
      working-directory: './terraform'
      check-name: 'terraform'
```

---

### MkDocs Build (`mkdocs-build.yml`)
Builds and validates MkDocs documentation.

**Inputs**:
```yaml
node-version:      '20'      # Node.js version
check-name:        'docs'    # Name of the check
```

**Usage**:
```yaml
jobs:
  docs:
    uses: logika-sciuro/.github/.github/workflows/mkdocs-build.yml@main
    with:
      node-version: '20'
      check-name: 'docs'
```

---

## Complete Example

Here's a complete workflow file that uses a shared workflow:

```yaml
# .github/workflows/build.yml (in your application repository)
name: Build

on:
  pull_request:
  push:
    branches: [main]

jobs:
  build:
    uses: logika-sciuro/.github/.github/workflows/java-build.yml@main
    with:
      java-version: '21'
      check-name: 'build'
```

---

## Versioning

Reference shared workflows using a branch, tag, or commit SHA:

- `@main` - Uses latest version (recommended for development)
- `@v1.0.0` - Uses specific version (recommended for production)
- `@abc1234` - Uses specific commit SHA

We recommend pinning to `@main` for now and transitioning to tagged versions when the workflows stabilize.

---

## Requirements for Consumer Repositories

When using these shared workflows, ensure your repository has:

### Java Projects
- `pom.xml` in repository root
- Maven commands: `clean`, `package`, `test`, `verify`

### Python Projects
- `requirements.txt` in repository root
- Optional: `pytest.ini` or tests in `tests/` directory

### Terraform Projects
- Terraform code in specified `working-directory`
- Optional: `.tfvars` files for variable configuration

### Documentation Projects
- `package.json` and `mkdocs.yml` in repository root
- `npm install` must install MkDocs (via `pip` or `npm` scripts)

---

## Troubleshooting

### Workflow Not Triggering
- Check branch protection rules - ensure `main` requires the check
- Verify workflow syntax with `yamllint`
- Check GitHub Actions logs for permissions errors

### Build Failures
- Check build logs in GitHub Actions tab
- Ensure repository has required configuration files
- Verify versions match repository requirements

### Cache Issues
- GitHub Actions caches are namespace by branch
- Clear cache if dependencies changed unexpectedly
- Cache TTL is 7 days by default

---

## Contributing

To add or modify shared workflows:

1. Make changes in `.github/.github/workflows/`
2. Test locally with [act](https://github.com/nektos/act) or in a test PR
3. Update this README with new inputs/examples
4. Update `SHARED_WORKFLOWS.md` with strategy changes

---

## Support

For questions or issues:
- Create an issue in `.github` repository with label `ci`
- Reference the workflow file name and error message
