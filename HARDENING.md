<!-- markdownlint-disable -->

# Hardening Report: EndBug--add-and-commit/v11.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **EndBug--add-and-commit/v11.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the referenced tag or branch is moved or compromised.

codeql-analysis.yml: actions/checkout@v7, github/codeql-action/init@v4, github/codeql-action/autobuild@v4, github/codeql-action/analyze@v4
dependency-review.yml: actions/checkout@v7, actions/dependency-review-action@v5
export-labels.yml: EndBug/export-label-config@main
label-sync.yml: actions/checkout@v7, EndBug/label-sync@v2
stale.yml: EndBug/workflows/.github/workflows/stale.yml@main
test.yml: actions/checkout@v7 (×3), actions/setup-node@v7 (×3), actions/upload-artifact@v7

Locations:

- `.github/workflows/codeql-analysis.yml:29`
- `.github/workflows/codeql-analysis.yml:33`
- `.github/workflows/codeql-analysis.yml:38`
- `.github/workflows/codeql-analysis.yml:48`
- `.github/workflows/dependency-review.yml:10`
- `.github/workflows/dependency-review.yml:12`
- `.github/workflows/export-labels.yml:11`
- `.github/workflows/label-sync.yml:14`
- `.github/workflows/label-sync.yml:15`
- `.github/workflows/stale.yml:8`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:36`
- `.github/workflows/test.yml:44`
- `.github/workflows/test.yml:46`
- `.github/workflows/test.yml:54`
- `.github/workflows/test.yml:56`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and no job-level `permissions:` blocks on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, which violates the principle of least privilege.

- export-labels.yml: no permissions defined at top level or job level
- label-sync.yml: no permissions defined at top level or job level
- stale.yml: no permissions defined at top level or job level

Locations:

- `.github/workflows/export-labels.yml:1`
- `.github/workflows/label-sync.yml:1`
- `.github/workflows/stale.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 10 distinct action references (17 total occurrences) across 6 workflow files to full 40-character commit SHAs with original tag/branch preserved as inline comments. Added minimal permissions blocks to the 3 workflows that lacked them: export-labels.yml (issues: read), label-sync.yml (issues: write), and stale.yml (issues: write, pull-requests: write).

