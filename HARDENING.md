<!-- markdownlint-disable -->

# Hardening Report: kishikawakatsumi--xcresulttool/v1.7.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kishikawakatsumi--xcresulttool/v1.7.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks. Affected references: actions/checkout@v3, actions/setup-node@v3.6.0, actions/upload-artifact@v3 in check-dist.yml; actions/checkout@v3, github/codeql-action/init@v2, github/codeql-action/autobuild@v2, github/codeql-action/analyze@v2 in codeql-analysis.yml; actions/checkout@v3 in test.yml.

Locations:

- `.github/workflows/check-dist.yml:22`
- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/codeql-analysis.yml:37`
- `.github/workflows/codeql-analysis.yml:42`
- `.github/workflows/codeql-analysis.yml:51`
- `.github/workflows/codeql-analysis.yml:58`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:20`

### missing-permissions (severity: medium)

check-dist.yml and test.yml have no top-level permissions: key and no job-level permissions: keys on any of their jobs. Without explicit permissions, workflows may run with overly broad default token permissions (read/write to repository contents). Only codeql-analysis.yml has explicit job-level permissions.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by replacing mutable tags with full 40-character SHA commit hashes (preserving original tags as comments). Added top-level `permissions: contents: read` to check-dist.yml and test.yml. codeql-analysis.yml already had appropriate job-level permissions (actions: read, contents: read, security-events: write) and only needed its action references pinned.

