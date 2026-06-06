# Labels and Milestones Synchronization

This document describes how to use the label and milestone synchronization workflows across repositories in the Logika-Sciuro organization.

## Overview

There are two main workflows:

- **Sync Repository Labels** — synchronizes labels across repositories
- **Sync Sprints (Milestones)** — synchronizes milestones across repositories

Both support three operational modes:

1. **Automatic Push** — triggered on merge to `main`, always in production mode (no dry-run)
2. **Workflow Dispatch (Dry-Run)** — test without modifying repositories
3. **Workflow Dispatch (Production)** — apply synchronization to real repositories

## Dry-Run Mode (Safe Testing)

Use this mode to **validate changes before applying** to production, without writing to any repository.

### Sync Repository Labels (Dry-Run)

1. Navigate to: `Actions` → `Sync Repository Labels`
2. Click **"Run workflow"**
3. Configure:
   - **target_env**: `sandbox` (default)
   - **dry_run**: `true` (default)
   - **repositories**: _(optional)_ list target repositories, e.g., `scavenger-infra-as-code, scavenger-core-gateway`
4. Click **"Run workflow"**

**Expected result:**
- The workflow validates that labels from `.github` are present in the target repositories
- If differences are found (missing, extra, color/description differences), reports warnings
- **No writes are performed**
- Status: ✓ success (no drift) or ✗ failure (drift detected)

**Example output:**
```
Dry-run validation passed for Logika-Sciuro/scavenger-infra-as-code.
Dry-run validation passed for Logika-Sciuro/scavenger-core-gateway.
```

Or with drift:
```
::warning::Missing label 'type: feature' in Logika-Sciuro/scavenger-infra-as-code.
::warning::Extra labels found in Logika-Sciuro/scavenger-infra-as-code:
bug
documentation
```

### Sync Sprints (Milestones) (Dry-Run)

1. Navigate to: `Actions` → `Sync Sprints (Milestones)`
2. Click **"Run workflow"**
3. Configure:
   - **target_env**: `sandbox` (default)
   - **dry_run**: `true` (default)
   - **repositories**: _(optional)_ e.g., `scavenger-infra-as-code`
4. Click **"Run workflow"**

**Expected result:**
- The workflow validates that milestones (Sprint 1/2/3) exist and are synchronized
- Reports warnings if differences exist in title, date, description, or state
- **No writes are performed**

**Example output:**
```
Milestone 'Sprint 1 - Fundação & Mocking Criptográfico' is already synchronized in Logika-Sciuro/scavenger-infra-as-code.
Milestone 'Sprint 2 - Caminho Quente (Multiplexação gRPC)' is already synchronized in Logika-Sciuro/scavenger-infra-as-code.
Milestone 'Sprint 3 - Caminho Frio & Validação E2E' is already synchronized in Logika-Sciuro/scavenger-infra-as-code.
```

## Production Mode

Use this mode to **synchronize real labels and milestones** across organization repositories.

### Sync Repository Labels (Production)

1. Navigate to: `Actions` → `Sync Repository Labels`
2. Click **"Run workflow"**
3. Configure:
   - **target_env**: `prod`
   - **dry_run**: `false`
   - **repositories**: _(leave empty to use default repositories)_
4. Click **"Run workflow"**

**Expected result:**
- The workflow synchronizes all labels from `.github` to target repositories
- Removes extra labels (not present in `.github/labels.yml`)
- Updates colors and descriptions of existing labels
- Status: ✓ success after synchronization

**Default repositories:**
- `scavenger-infra-as-code`
- `scavenger-core-gateway`
- `scavenger-cv-engine`
- `scavenger-e2e-framework`

### Sync Sprints (Milestones) (Production)

1. Navigate to: `Actions` → `Sync Sprints (Milestones)`
2. Click **"Run workflow"**
3. Configure:
   - **target_env**: `prod`
   - **dry_run**: `false`
   - **repositories**: _(leave empty to use default repositories)_
4. Click **"Run workflow"**

**Expected result:**
- The workflow creates or updates the 3 sprint milestones:
  - Sprint 1 - Fundação & Mocking Criptográfico (due: 2026-05-21)
  - Sprint 2 - Caminho Quente (Multiplexação gRPC) (due: 2026-05-28)
  - Sprint 3 - Caminho Frio & Validação E2E (due: 2026-06-04)
- Status: ✓ success after synchronization

## Recommended Workflow

### For Development/Testing

1. Modify `.github/labels.yml` or the milestones workflow locally
2. Create a PR and request review
3. **Before merging**, run dry-run on the branch:
   - Dispatch with `target_env=prod`, `dry_run=true`
   - Check for unexpected warnings
4. After approval, merge to `main`
5. The production workflow runs automatically (no dry-run)

### For Quick Synchronization

If you're confident and tests have passed:

1. Dispatch with `target_env=prod`, `dry_run=false`
2. Wait for completion
3. Manually verify one repository:
   ```bash
   gh label list -R Logika-Sciuro/scavenger-infra-as-code
   gh api /repos/Logika-Sciuro/scavenger-infra-as-code/milestones --jq '.[].title'
   ```

## Input Reference

| Input | Type | Default | Description |
|-------|------|---------|-----------|
| `target_env` | choice | `sandbox` | Target environment: `sandbox` or `prod` |
| `dry_run` | boolean | `true` | Validate without writing |
| `repositories` | string | _(empty)_ | Repositories (comma-separated, without owner). If empty and `target_env=prod`, uses default. If empty and `target_env=sandbox`, requires input. |

## Troubleshooting

### Error: "No repositories resolved"

**Cause:** Executed with `target_env=sandbox` without providing `repositories`.

**Solution:** Provide a list of repositories in `repositories`, e.g., `sandbox-labels-test, my-test-repo`.

### Dry-run detects drift, but production fails

**Cause:** There may be uncommitted changes or the workflow is not updated on remote.

**Solution:**
1. Verify the branch is up to date: `git pull origin main`
2. Confirm changes are in `.github/labels.yml`
3. Rerun the dry-run

### Labels do not appear after production sync

**Cause:** Token lacks adequate permissions or rate-limit exceeded.

**Solution:**
1. Confirm `ORG_PAT_ISSUES_WRITE` has scopes: `repo`, `issues`
2. Wait a few minutes and try again
3. Verify target repository permissions: Organization owner should have access

### Milestones did not synchronize correctly

**Cause:** Date formats or state incompatibilities.

**Solution:**
1. Verify expected dates in `sync-sprints-milestones.yml`
2. Validate via API:
   ```bash
   gh api /repos/Logika-Sciuro/scavenger-infra-as-code/milestones --jq '.[] | {title, state, due_on}'
   ```

## Useful Links

- [Workflow: Sync Repository Labels](.github/workflows/sync-labels.yml)
- [Workflow: Sync Sprints (Milestones)](.github/workflows/sync-sprints-milestones.yml)
- [Labels Config](.github/labels.yml)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
