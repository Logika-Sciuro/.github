# Versioning & Release Policy

This document defines versioning, release, and changelog standards for all Logika-Sciuro repositories. It ensures consistency, traceability, and clear communication about changes across the platform.

---

## Versioning Standard: Semantic Versioning (SemVer)

All repositories follow [Semantic Versioning 2.0.0](https://semver.org/).

### Version Format

```
MAJOR.MINOR.PATCH[-PRERELEASE][+BUILD]
```

Example: `1.2.3`, `2.0.0-beta.1`, `1.0.0+build.12345`

### When to Increment

| Scenario | Increment | Example |
|---|---|---|
| Bug fixes, internal improvements | PATCH | 1.2.3 → 1.2.4 |
| New features (backward compatible) | MINOR | 1.2.4 → 1.3.0 |
| Breaking changes (incompatible) | MAJOR | 1.3.0 → 2.0.0 |
| Pre-release (beta, RC, etc.) | PRERELEASE | 2.0.0-beta.1 |

### Rules

1. **MAJOR version** when you make incompatible API changes
   - Java: Remove/rename public classes, methods, or change signatures
   - Python: Break gRPC service contract or HTTP API
   - IaC: Destroy or fundamentally change infrastructure resources
   - Docs: Significant restructuring (move/delete major sections)

2. **MINOR version** when you add functionality in a backward-compatible manner
   - New endpoints, methods, or features
   - Deprecations (with deprecation notice)
   - New optional configuration

3. **PATCH version** for backward-compatible bug fixes
   - Fix logic errors
   - Performance improvements
   - Documentation corrections

4. **PRERELEASE** for testing before release
   - Format: `1.0.0-alpha`, `1.0.0-beta.1`, `1.0.0-rc.2`
   - Sorted numerically, so `-rc.2` > `-rc.1` > `-beta`

---

## Release Workflow

### Phase 1: Prepare Release Branch

When ready to release (all features merged, tests passing):

```bash
# Create release branch from main
git checkout main
git pull origin main
git checkout -b release/v1.2.0  # Use version number

# Or, for patch releases:
git checkout -b release/v1.2.1
```

### Phase 2: Update Version and Changelog

**Update version in project files:**

#### Java
- Update `<version>` in `pom.xml` to `1.2.0` (remove -SNAPSHOT if present)
- Rebuild and verify: `mvn clean verify`

#### Python
- Update `version` in `pyproject.toml` or `setup.py`
- Update version string in source if applicable
- Rebuild: `pip install -e .`

#### Terraform/IaC
- Document version in `terraform/versions.tf` or README
- Tag releases consistently (no internal version file needed)

#### Docs
- Update version in `mkdocs.yml` (if applicable)
- Rebuild: `mkdocs build`

**Update CHANGELOG.md** (in all repos):

```markdown
## [1.2.0] - 2026-06-15

### Added
- New feature X (scavenger-core-gateway)
- New endpoint /api/v2/users (breaking version 2.0)

### Changed
- Improved performance of job processing (15% faster)
- Updated dependencies: Maven 3.9.5 → 3.9.6

### Fixed
- Fixed race condition in concurrent job handling
- Fixed documentation typo in API guide

### Deprecated
- Endpoint `/api/v1/users` (use `/api/v2/users` instead, will be removed in 2.0)

### Security
- Fixed XSS vulnerability in HTML rendering

### Breaking Changes (if MAJOR version)
- Java: `JobProcessor.execute()` renamed to `execute(JobRequest)`
- Endpoints responding with version 1 JSON schema migrated to v2

### Migration Guide (if MAJOR version)
See UPGRADE_v1_to_v2.md for detailed migration steps.

### Contributors
- @alice (feature X)
- @bob (performance improvements)
```

### Phase 3: Open Release PR

Create a PR from `release/v1.2.0` to `main`:

```bash
git push origin release/v1.2.0
gh pr create --head release/v1.2.0 --base main \
  --title "chore(release): v1.2.0" \
  --body "Release v1.2.0 — see CHANGELOG.md for details"
```

**PR Checklist:**
- [ ] Version bumped in all project files
- [ ] CHANGELOG.md updated with all changes
- [ ] Breaking changes documented (if MAJOR)
- [ ] Migration guide provided (if MAJOR)
- [ ] CI/CD passes
- [ ] Code review approved

### Phase 4: Merge Release PR

After PR approval and all checks pass:

```bash
# Merge to main (use --squash for clean history, or --merge for full history)
gh pr merge <pr_number> --merge
```

### Phase 5: Tag Release

After merging to main, create a release tag:

```bash
# Pull latest main
git fetch origin
git checkout main
git pull origin main

# Create annotated tag
git tag -a v1.2.0 -m "Release v1.2.0

- New feature X
- Performance improvements
- Bug fixes

See CHANGELOG.md for full details."

# Push tag to remote
git push origin v1.2.0
```

### Phase 6: Create Release Notes

On GitHub, create a release from the tag:

```bash
gh release create v1.2.0 --title "v1.2.0" \
  --notes-file CHANGELOG.md
```

Or manually on GitHub:
1. Go to Releases → Draft a new release
2. Select tag `v1.2.0`
3. Title: "v1.2.0 — [Feature description]"
4. Description: Copy CHANGELOG section for this version
5. Publish release

---

## Changelog Format

Follow [Keep a Changelog](https://keepachangelog.com/) format for consistency.

### Template

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- (upcoming changes)

## [1.2.0] - 2026-06-15

### Added
- New feature X

### Changed
- Updated dependency Y

### Fixed
- Fixed bug Z

### Deprecated
- Old API endpoint (will be removed in 2.0)

### Security
- Fixed security issue

### Breaking Changes
- Removed deprecated field X (use Y instead)

## [1.1.0] - 2026-05-01

### Added
- Feature A
```

### Guidelines

- **Added** — New features (always backward compatible)
- **Changed** — Changes in existing functionality (backward compatible)
- **Deprecated** — Features no longer recommended (but still work)
- **Removed** — Features deleted in this version (BREAKING)
- **Fixed** — Bug fixes (backward compatible)
- **Security** — Security vulnerability fixes

### Automation Hint

Use commit messages to auto-generate changelog entries:

```bash
# Use semantic commit types to categorize changes
# feat → Added
# fix → Fixed
# docs → (docs changelog, if separate)
# perf → Performance improvements
# security → Security

# Example: commits in release v1.2.0
git log v1.1.0..v1.2.0 --oneline --grep="^feat\|^fix\|^security"
```

---

## Repository-Specific Considerations

### Java Repositories (`scavenger-core-gateway`, `scavenger-e2e-framework`)

**Version Management:**
- Maven uses `<version>` in `pom.xml`
- During development: `1.2.0-SNAPSHOT`
- During release: `1.2.0` (no -SNAPSHOT)
- After release: `1.3.0-SNAPSHOT` (next dev version)

**Release Command:**
```bash
# Prepare release
mvn release:prepare -DautoVersionSubmodules=true

# Perform release (creates tag, bumps version)
mvn release:perform

# Or manually:
mvn versions:set -DnewVersion=1.2.0
git add pom.xml && git commit -m "chore(release): set version 1.2.0"
git tag v1.2.0
mvn versions:set -DnewVersion=1.3.0-SNAPSHOT
git add pom.xml && git commit -m "chore: set next dev version 1.3.0-SNAPSHOT"
```

**Artifact Management:**
- Snapshots: Published to organization Maven repository (dev use only)
- Releases: Published to Maven Central (public)

---

### Python Repository (`scavenger-cv-engine`)

**Version Management:**
- Python uses version in `pyproject.toml` or `setup.py`
- During development: `1.2.0.dev0`
- During release: `1.2.0`
- After release: `1.3.0.dev0` (next dev version)

**Release Command:**
```bash
# Update version
# In pyproject.toml: version = "1.2.0"
# In setup.py (if used): version="1.2.0"

# Build distribution
python -m build

# Tag release
git tag v1.2.0

# Upload to PyPI (if publishing publicly)
python -m twine upload dist/scavenger_cv_engine-1.2.0*

# Or to private package registry
pip install .  # Local test
```

---

### Infrastructure Repository (`scavenger-infra-as-code`)

**Version Management:**
- IaC versions don't require code changes, only tags
- Version reflects infrastructure schema/module version
- Versioning is documented in `versions.tf` comments

**Release Pattern:**
```bash
# Tag infrastructure release
git tag v1.2.0 -m "Infrastructure schema v1.2.0

- Added VPC module v2
- Updated security groups
- See CHANGELOG.md for details"

git push origin v1.2.0
```

**Backward Compatibility:**
- Terraform modules should maintain backward compatibility
- Use optional variables with sensible defaults
- Breaking changes → MAJOR version, include migration docs

---

### Documentation Repository (`docs`)

**Version Management:**
- Docs version tied to platform releases
- Maintained in `mkdocs.yml` (site_name and version)
- Follows platform versioning (e.g., v1.2.0)

**Release Pattern:**
- Tag with same version as corresponding platform release
- Include in release notes the platform versions documented

---

## Pre-Release Workflow

For testing before stable release:

### Alpha/Beta Release

```bash
# Create pre-release tag
git tag v1.2.0-alpha.1 -m "Alpha release for testing"
git push origin v1.2.0-alpha.1

# Or on GitHub
gh release create v1.2.0-alpha.1 --prerelease --title "v1.2.0-alpha.1" \
  --notes "Test release — DO NOT use in production"
```

### Release Candidates (RC)

```bash
# Release candidate before final release
git tag v1.2.0-rc.1 -m "Release candidate 1"
git push origin v1.2.0-rc.1

# After feedback, rc.2, rc.3, etc.
git tag v1.2.0-rc.2 -m "Release candidate 2 — fixes from rc.1"
git push origin v1.2.0-rc.2
```

---

## Dependency Updates

When updating dependencies, follow these guidelines:

### Patch-Level Updates (e.g., 3.8.1 → 3.8.2)
- Use PATCH version bump
- Document in CHANGELOG under "Changed" section
- Automated dependency management tools are acceptable

### Minor-Level Updates (e.g., 3.8.x → 3.9.x)
- Usually PATCH or MINOR version bump (depending on breaking changes)
- Review compatibility
- Document in CHANGELOG

### Major-Level Updates (e.g., 3.x.x → 4.x.x)
- Usually requires MAJOR version bump
- Review breaking changes in dependency
- Provide migration guide in CHANGELOG

---

## Long-Term Support (LTS) Versions

For critical infrastructure or long-lived platforms:

```
v2.0.0 (LTS)
↓ (receive backport updates)
v2.0.1, v2.0.2, v2.0.3, ...
↓ (end of support after 12 months)

v3.0.0 (latest)
↓ (active development)
```

For now, all repositories track the latest version. LTS policy is deferred to future phase.

---

## Release Cadence

Recommended release schedule:

| Repository | Cadence | Rationale |
|---|---|---|
| scavenger-core-gateway | Every 2 weeks | Core platform component |
| scavenger-cv-engine | Every 2-4 weeks | Feature-driven |
| scavenger-e2e-framework | Every 2-4 weeks | Test/QA driven |
| scavenger-infra-as-code | As needed | Reactive to platform changes |
| docs | Per platform release | Synchronized with platform |

Exceptions:
- Security fixes → release ASAP
- Critical bugs → expedited release cycle

---

## Commit Message Best Practices

For accurate changelog generation:

**Use semantic commits to convey change type:**

```
feat(api): add user profile endpoint        → Added section
fix(db): prevent connection leak             → Fixed section
perf(cache): improve cache eviction          → Changed section
security(auth): fix JWT validation           → Security section
docs(readme): clarify installation steps      → (docs only)
chore(deps): update Maven to 3.9.6           → Changed section
refactor(core): simplify business logic      → Changed section
```

**Include scope and description:**

```
good:   feat(api): add /users/profile endpoint
bad:    feat: add endpoint
worse:  add endpoint
```

**In commit body, reference issues and explain "why":**

```
feat(api): add /users/profile endpoint

Implements new endpoint for retrieving user profile information.
Supports optional profile customization via query parameters.

Fixes #123
Relates to architecture ADR-001 (User Profile Schema)
```

---

## FAQ

**Q: Do we use `v` prefix in versions?**
- Tag names: Yes (`v1.2.0`)
- Version fields in code: No (`1.2.0`)
- This makes GitHub releases and docs consistent

**Q: What if we discover a bug in a released version?**
- Create a new PATCH release with the fix
- If version is 1.2.0, release is 1.2.1
- Tag as v1.2.1, update CHANGELOG, create release

**Q: Can we skip CHANGELOG entries for small changes?**
- No. All changes visible to users or developers should be documented
- Even small fixes help users understand what changed

**Q: When do we release?**
- When features are merged, tested, and ready
- Before release: ensure all tests pass and documentation is current
- At least 1 approval required (per MERGE_GATES.md)

**Q: Can we use different versioning for different repos?**
- Not recommended. Consistency aids users and deployment
- If a repo is decoupled (separate domain), it can have its own version
- Coordinated releases are preferred

---

## Tools & Automation

**Recommended tools for automation (future):**
- Commitlint: Enforce semantic commit format ✅ (already using)
- Conventional Commits: Parse commits for changelog generation
- Semantic Release: Automate versioning, tags, and CHANGELOG
- Changelog generator: Generate CHANGELOG from commits

---

## References

- [Semantic Versioning 2.0.0](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [MERGE_GATES.md](./MERGE_GATES.md) — Merge approval criteria
- [SHARED_WORKFLOWS.md](./SHARED_WORKFLOWS.md) — CI/CD pipeline details

