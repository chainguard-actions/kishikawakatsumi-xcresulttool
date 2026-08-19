<!-- markdownlint-disable -->

# Hardening Report: kishikawakatsumi--xcresulttool/v1.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kishikawakatsumi--xcresulttool/v1.7.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use action references pinned to mutable tags/versions rather than immutable 40-character commit SHAs, making them vulnerable to supply-chain attacks.

.github/workflows/check-dist.yml: actions/checkout@v3, actions/setup-node@v3.5.1, actions/upload-artifact@v3

.github/workflows/codeql-analysis.yml: actions/checkout@v3, github/codeql-action/init@v2, github/codeql-action/autobuild@v2, github/codeql-action/analyze@v2

.github/workflows/test.yml: actions/checkout@v3

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/codeql-analysis.yml:31`
- `.github/workflows/codeql-analysis.yml:36`
- `.github/workflows/codeql-analysis.yml:44`
- `.github/workflows/codeql-analysis.yml:52`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:19`

### missing-permissions (severity: medium)

test.yml and check-dist.yml have no top-level permissions: key and no per-job permissions: key on any of their jobs. Without explicit permissions, jobs inherit the default repository permissions (which may be broad write access), violating the principle of least privilege.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/check-dist.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full commit SHAs: actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3.5.1 → 8c91899e586c5b171469028077307d293428b516, actions/upload-artifact@v3 → ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5, github/codeql-action/{init,autobuild,analyze}@v2 → b8d3b6e8af63cde30bdc382c0bc28114f4346c88. Added top-level `permissions: contents: read` to check-dist.yml and test.yml. codeql-analysis.yml already had explicit per-job permissions (actions: read, contents: read, security-events: write) so no change was needed there.

