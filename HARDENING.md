<!-- markdownlint-disable -->

# Hardening Report: EndBug--add-and-commit/v10.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **EndBug--add-and-commit/v10.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of pinned full 40-character commit SHAs. This exposes the workflow to supply-chain attacks where a tag or branch can be silently updated to point to malicious code.

Failing references:
- codeql-analysis.yml: actions/checkout@v6, github/codeql-action/init@v4, github/codeql-action/autobuild@v4, github/codeql-action/analyze@v4
- dependency-review.yml: actions/checkout@v6, actions/dependency-review-action@v4
- export-labels.yml: EndBug/export-label-config@main
- label-sync.yml: actions/checkout@v6, EndBug/label-sync@v2
- stale.yml: EndBug/workflows/.github/workflows/stale.yml@main
- test.yml: actions/checkout@v6 (×3), actions/setup-node@v6 (×3)
- versioning.yml: Actions-R-Us/actions-tagger@v2

Locations:

- `.github/workflows/codeql-analysis.yml:30`
- `.github/workflows/codeql-analysis.yml:34`
- `.github/workflows/codeql-analysis.yml:39`
- `.github/workflows/codeql-analysis.yml:44`
- `.github/workflows/dependency-review.yml:11`
- `.github/workflows/dependency-review.yml:13`
- `.github/workflows/export-labels.yml:10`
- `.github/workflows/label-sync.yml:14`
- `.github/workflows/label-sync.yml:15`
- `.github/workflows/stale.yml:8`
- `.github/workflows/test.yml:12`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:22`
- `.github/workflows/test.yml:23`
- `.github/workflows/test.yml:32`
- `.github/workflows/test.yml:33`
- `.github/workflows/versioning.yml:10`

### missing-permissions (severity: medium)

Five workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (which may be broad, e.g. write access to contents). Each of these files should declare minimal required permissions.

- export-labels.yml: no permissions declared at top-level or job level
- label-sync.yml: no permissions declared at top-level or job level
- stale.yml: no permissions declared at top-level or job level
- test.yml: no permissions declared at top-level or job level (3 jobs: build, test, lint)
- versioning.yml: no permissions declared at top-level or job level

Locations:

- `.github/workflows/export-labels.yml:1`
- `.github/workflows/label-sync.yml:1`
- `.github/workflows/stale.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 17 unpinned action references across 7 workflow files by pinning each to its full 40-character commit SHA (with original tag preserved as a comment). Added minimal permissions blocks to the 5 workflow files that were missing them: export-labels.yml (issues: read), label-sync.yml (issues: write), stale.yml (issues: write + pull-requests: write), test.yml (contents: read), and versioning.yml (contents: write). The dependency-review.yml and codeql-analysis.yml already had permissions blocks and only needed their action references pinned.

