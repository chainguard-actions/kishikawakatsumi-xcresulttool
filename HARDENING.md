<!-- markdownlint-disable -->

# Hardening Report: kishikawakatsumi--xcresulttool/v1.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kishikawakatsumi--xcresulttool/v1.5.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use tag-based or version-based `uses:` references instead of pinned full-length SHA commits, making them vulnerable to supply-chain attacks if the referenced tag is moved.

- `.github/workflows/test.yml`: `actions/checkout@v3` (appears twice)
- `.github/workflows/check-dist.yml`: `actions/checkout@v3`, `actions/setup-node@v3.4.1`, `actions/upload-artifact@v3`
- `.github/workflows/codeql-analysis.yml`: `actions/checkout@v3`, `github/codeql-action/init@v2`, `github/codeql-action/autobuild@v2`, `github/codeql-action/analyze@v2`

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:21`
- `.github/workflows/check-dist.yml:22`
- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:46`
- `.github/workflows/codeql-analysis.yml:32`
- `.github/workflows/codeql-analysis.yml:37`
- `.github/workflows/codeql-analysis.yml:46`
- `.github/workflows/codeql-analysis.yml:56`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, workflows run with the default (potentially broad) token permissions.

- `.github/workflows/test.yml`: Neither the workflow nor its `build` or `test` jobs declare `permissions:`.
- `.github/workflows/check-dist.yml`: Neither the workflow nor its `check-dist` job declares `permissions:`.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/check-dist.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving full commit SHAs via lookup_action_sha and updating all three workflow files. Added top-level `permissions: contents: read` to test.yml and check-dist.yml to address missing-permissions findings. The codeql-analysis.yml already had job-level permissions (actions: read, contents: read, security-events: write) so no permissions change was needed there.

