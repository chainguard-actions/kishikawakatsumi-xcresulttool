<!-- markdownlint-disable -->

# Hardening Report: kishikawakatsumi--xcresulttool/v1.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kishikawakatsumi--xcresulttool/v1.6.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use mutable tag-based refs instead of pinned 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced action tags are moved.

.github/workflows/check-dist.yml:
- uses: actions/checkout@v3
- uses: actions/setup-node@v3.4.1
- uses: actions/upload-artifact@v3

.github/workflows/codeql-analysis.yml:
- uses: actions/checkout@v3
- uses: github/codeql-action/init@v2
- uses: github/codeql-action/autobuild@v2
- uses: github/codeql-action/analyze@v2

.github/workflows/test.yml:
- uses: actions/checkout@v3 (appears twice)

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/codeql-analysis.yml:30`
- `.github/workflows/codeql-analysis.yml:35`
- `.github/workflows/codeql-analysis.yml:42`
- `.github/workflows/codeql-analysis.yml:47`
- `.github/workflows/test.yml:12`
- `.github/workflows/test.yml:19`

### missing-permissions (severity: medium)

check-dist.yml and test.yml have no top-level permissions: key and no job-level permissions: keys on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions. Each workflow should declare minimal required permissions.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all mutable action refs to full 40-char SHAs in all three workflow files: actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3.4.1 → @2fddd8803e2f5c9604345a0b591c3020ee971a93, actions/upload-artifact@v3 → @ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5, github/codeql-action/{init,autobuild,analyze}@v2 → @b8d3b6e8af63cde30bdc382c0bc28114f4346c88. Added top-level 'permissions: contents: read' to check-dist.yml and test.yml (codeql-analysis.yml already had job-level permissions). Original tag names preserved as inline comments.

