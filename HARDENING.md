<!-- markdownlint-disable -->

# Hardening Report: kishikawakatsumi--xcresulttool/v1.5.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kishikawakatsumi--xcresulttool/v1.5.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files use tag-based or version-based refs instead of full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the tag is moved.

- .github/workflows/test.yml: `actions/checkout@v3`
- .github/workflows/check-dist.yml: `actions/checkout@v3`, `actions/setup-node@v3.4.1`, `actions/upload-artifact@v3`
- .github/workflows/codeql-analysis.yml: `actions/checkout@v3`, `github/codeql-action/init@v2`, `github/codeql-action/autobuild@v2`, `github/codeql-action/analyze@v2`

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:44`
- `.github/workflows/codeql-analysis.yml:33`
- `.github/workflows/codeql-analysis.yml:38`
- `.github/workflows/codeql-analysis.yml:46`
- `.github/workflows/codeql-analysis.yml:56`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` key and no job-level `permissions:` keys, meaning jobs run with the default (potentially broad) GITHUB_TOKEN permissions.

- .github/workflows/test.yml: Neither the `build` job nor the `test` job defines permissions.
- .github/workflows/check-dist.yml: The `check-dist` job does not define permissions.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/check-dist.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. **unpinned-uses**: Pinned all action references to full 40-character SHA hashes:
   - `actions/checkout@v3` → `@f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3` (in test.yml, check-dist.yml, codeql-analysis.yml)
   - `actions/setup-node@v3.4.1` → `@2fddd8803e2f5c9604345a0b591c3020ee971a93 # v3.4.1` (in check-dist.yml)
   - `actions/upload-artifact@v3` → `@ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5 # v3` (in check-dist.yml)
   - `github/codeql-action/init@v2` → `@b8d3b6e8af63cde30bdc382c0bc28114f4346c88 # v2` (in codeql-analysis.yml)
   - `github/codeql-action/autobuild@v2` → `@b8d3b6e8af63cde30bdc382c0bc28114f4346c88 # v2` (in codeql-analysis.yml)
   - `github/codeql-action/analyze@v2` → `@b8d3b6e8af63cde30bdc382c0bc28114f4346c88 # v2` (in codeql-analysis.yml)

2. **missing-permissions**: Added `permissions: {}` at the top level of test.yml and check-dist.yml. The codeql-analysis.yml already had appropriate job-level permissions (actions: read, contents: read, security-events: write).

