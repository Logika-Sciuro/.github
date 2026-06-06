# Merge Gates Policy

This document defines the mandatory merge gates (acceptance criteria) for pull requests across Logika-Sciuro repositories. These gates ensure code quality, consistency, and traceability without relying on branch protection (which is deferred to a later phase).

**Status**: This policy is enforced through team discipline and PR review workflows, **not** automated branch protection rules.

---

## Overview

A PR is ready to merge when **all** of the following are satisfied:

1. ✅ **Code Review** — Mandatory review and approval
2. ✅ **Checks** — CI/CD pipeline passes
3. ✅ **Documentation** — Changes documented appropriately
4. ✅ **Merge Conflict** — Branch is mergeable with no conflicts
5. ✅ **Semantic Commits** — Message follows conventional commits

---

## Merge Gate Details

### 1. Code Review (All Repositories)

**Mandatory Review Criteria:**
- Minimum **1 approval** from a team member (not the author)
- Approval must address the change's correctness and fit
- For architectural/design changes or public APIs: escalate to tech lead

**What Reviewers Check:**
- ✅ Code correctness and logic
- ✅ No security vulnerabilities or credential leaks
- ✅ Performance implications considered
- ✅ Error handling and edge cases
- ✅ Consistency with codebase conventions
- ✅ Tests cover behavior, not just code coverage

**Review Workflow:**
1. Author opens PR and requests review
2. Reviewer examines changes and comments inline if needed
3. Author addresses feedback or explains rationale
4. Reviewer approves when satisfied
5. Author merges after all gates pass

---

### 2. Checks (CI/CD Passes)

All required CI checks must pass on the merge commit.

#### Java Repositories (`scavenger-core-gateway`, `scavenger-e2e-framework`)

**Required Checks:**
- ✅ Build (`mvn clean package`)
- ✅ Tests (`mvn test`)
- ✅ Quality (`mvn verify`)
- ✅ Commit lint (`commitlint`)

**Failure Resolution:**
- Investigate logs to identify root cause
- Fix the issue in the code, not the build (no disabling checks)
- Push fix and re-run CI
- Merge only after all checks pass

---

#### Python Repository (`scavenger-cv-engine`)

**Required Checks:**
- ✅ Lint (`flake8`)
- ✅ Tests (`pytest`)
- ✅ Type hints validation (if configured)
- ✅ Commit lint (`commitlint`)

**Failure Resolution:**
- Same as Java — fix the code, not the checks

---

#### Documentation Repository (`docs`)

**Required Checks:**
- ✅ MkDocs build (`mkdocs build --strict`)
- ✅ Commit lint (`commitlint`)
- ✅ No broken links (if configured)

**Failure Resolution:**
- Fix documentation or configuration, rebuild locally
- Verify `site/` builds without errors
- Push and re-run CI

---

#### Infrastructure Repository (`scavenger-infra-as-code`)

**Required Checks:**
- ✅ Terraform format (`terraform fmt -check`)
- ✅ Terraform validate (`terraform validate`)
- ✅ Terraform plan (no errors, review plan)
- ✅ Commit lint (`commitlint`)

**Failure Resolution:**
- Run `terraform fmt` locally to fix formatting
- Run `terraform validate` to catch configuration errors
- Review `terraform plan` for unintended changes
- Push and re-run CI

---

### 3. Documentation (Applies to All Changes)

**Requirements:**

| Change Type | Documentation Expected |
|---|---|
| New API endpoint | Update API docs or inline code comments |
| New configuration property | Update config docs (config.md or similar) |
| Breaking change | Update CHANGELOG.md + migration guide |
| New feature | Update README.md feature list or docs site |
| Bug fix | Reference issue in commit message; add to CHANGELOG |
| Refactor | Code comments if logic is non-obvious |
| Dependency upgrade | Note in CHANGELOG if public-facing impact |

**Checklist:**
- [ ] Code changes are self-documenting or commented
- [ ] README.md updated if relevant
- [ ] CHANGELOG.md updated for user-facing changes
- [ ] Inline comments explain "why," not "what"
- [ ] No commented-out code (delete or explain)

---

### 4. Mergeable Branch

**Requirements:**
- ✅ Branch has no merge conflicts with target (`main`)
- ✅ Branch is not behind target (when required by repo)
- ✅ All commits are properly attributed

**Before Merge:**
```bash
# Verify branch is mergeable
git fetch origin
git merge-base --is-ancestor origin/main HEAD  # Should succeed

# Or in GitHub UI, check that PR shows "No conflicts"
```

**If Behind:**
- Rebase or merge target into branch
- Re-run CI to validate final state
- Then merge

---

### 5. Semantic Commits

**Requirement:**
All commits must follow [conventional commits](../CONTRIBUTING.md) format.

**Format:**
```
<type>(<scope>): <description>

<optional body>

<optional footer>
```

**Examples:**
```
feat(java-build): add support for custom Maven arguments
fix(python-build): correct flake8 configuration for F841
docs(merge-gates): add approval criteria
test(terraform-validate): add validation test cases
chore(deps): update GitHub Actions to latest versions
```

**Verification:**
- ✅ Commit lint CI check passes
- ✅ Manual review: commit messages are clear and semantic

---

## Repository-Specific Policies

### Java Repositories

**Additional Requirements:**
- ✅ Tests have >60% code coverage (minimum)
- ✅ No `@Suppress` or `@SuppressWarnings` without justification
- ✅ All public methods/classes have Javadoc

**Code Review Checklist:**
- [ ] Thread safety reviewed (if concurrent code)
- [ ] Exception handling is complete
- [ ] Resource cleanup (file handles, connections)

---

### Python Repository

**Additional Requirements:**
- ✅ Type hints on public functions (Python 3.11+)
- ✅ Docstrings on public functions (Google-style or NumPy)
- ✅ Tests cover happy path and edge cases

**Code Review Checklist:**
- [ ] No mutable defaults in function signatures
- [ ] Async/await patterns are correct (if FastAPI)
- [ ] gRPC service definitions are complete

---

### Documentation Repository

**Additional Requirements:**
- ✅ All pages have meaningful titles and navigation
- ✅ Links are working (build validates them)
- ✅ Admonitions (Note, Warning, Tip) are used appropriately

**Code Review Checklist:**
- [ ] No orphaned pages (all linked from nav)
- [ ] Screenshots/diagrams are current
- [ ] Code examples are executable/tested

---

### Infrastructure Repository

**Additional Requirements:**
- ✅ Plan shows only intended changes (no accidental mutations)
- ✅ Sensitive values (passwords, keys) are never logged
- ✅ Backward compatibility maintained for existing infrastructure

**Code Review Checklist:**
- [ ] Review Terraform plan output for correctness
- [ ] Cost implications noted (if applicable)
- [ ] No hardcoded values — use variables/locals
- [ ] Modules are reusable and well-scoped

---

## Approval Flow (Without Branch Protection)

Since branch protection rules are not yet enabled, approvals are **voluntary discipline**:

### Process

1. **Author**
   - Opens PR with clear description
   - Ensures all checks pass
   - Requests review from appropriate team member

2. **Reviewer**
   - Reviews code thoroughly
   - Comments inline on issues
   - Approves via GitHub when satisfied
   - Does **not** merge (author's responsibility)

3. **Author**
   - Addresses review feedback
   - Pushes fixes and re-runs CI
   - Merges **after** all gates pass and review is approved

### Enforcement

**For now, enforcement is via:**
- Team discipline and code review culture
- Audit trail in commit history (semantic commits + sign-offs)
- CI/CD visibility (checks must pass)

**In a future phase:**
- Automated branch protection rules
- CODEOWNERS file for mandatory reviewers
- Merge automation based on approval state

---

## Evidence and Audit Trail

Each merge should have clear evidence:

### In GitHub PR/Commit:
- ✅ Semantic commit message
- ✅ At least one approval (via GitHub "Approve" review)
- ✅ All CI checks passing
- ✅ Co-authored-by trailer (if collaborative work)

### Example:
```
feat(terraform): add VPC module with security groups

- Create reusable VPC module with configurable subnets
- Add security group module with ingress/egress rules
- Document module inputs and outputs in README

Terraform plan verified: 3 resources added, 0 modified, 0 destroyed

Co-authored-by: Jane Doe <jane@example.com>

Reviewed-by: John Smith <john@example.com>
```

---

## FAQ

**Q: What if I disagree with review feedback?**
- Reply to the comment explaining your rationale
- If reviewer insists, escalate to tech lead or team discussion
- Goal: resolve via dialogue, not override

**Q: Can I merge my own PR?**
- No. Minimum 1 approval from another team member required
- Exception: Critical hotfixes (document in PR body)

**Q: What if CI fails for external reasons (flaky test, infrastructure down)?**
- Investigate to confirm it's not a code issue
- Retry CI and see if it passes on next run
- If persistently flaky, file a separate issue to fix the test/infra
- Do **not** disable the check to unblock merge

**Q: Can we skip documentation for small changes?**
- Depends on change scope. If it affects user behavior or API, document it
- When in doubt, include a comment in the PR explaining rationale

**Q: When do branch protection rules get enabled?**
- Future phase (Phase 3 in the roadmap)
- Timing depends on team size and repository setup
- Documentation here prepares us for that transition

---

## References

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning (SemVer)](https://semver.org/)
- [SHARED_WORKFLOWS.md](./SHARED_WORKFLOWS.md) — CI/CD pipeline details
- [CONTRIBUTING.md](./CONTRIBUTING.md) — Contribution guidelines

