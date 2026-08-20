<!-- markdownlint-disable -->

# Hardening Report: EndBug--add-and-commit/v11.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **EndBug--add-and-commit/v11.1.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tag or branch refs instead of immutable full-length SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced tag or branch is moved.

Failing references:
- codeql-analysis.yml: actions/checkout@v7, github/codeql-action/init@v4, github/codeql-action/autobuild@v4, github/codeql-action/analyze@v4
- dependabot-rebuild.yml: actions/checkout@v7, actions/setup-node@v7
- dependency-review.yml: actions/checkout@v7, actions/dependency-review-action@v5
- export-labels.yml: EndBug/export-label-config@main
- label-sync.yml: actions/checkout@v7, EndBug/label-sync@v2
- stale.yml: EndBug/workflows/.github/workflows/stale.yml@main
- test.yml: actions/checkout@v7, actions/setup-node@v7, actions/upload-artifact@v7

Locations:

- `.github/workflows/codeql-analysis.yml:27`
- `.github/workflows/codeql-analysis.yml:32`
- `.github/workflows/codeql-analysis.yml:40`
- `.github/workflows/codeql-analysis.yml:52`
- `.github/workflows/dependabot-rebuild.yml:16`
- `.github/workflows/dependabot-rebuild.yml:21`
- `.github/workflows/dependency-review.yml:9`
- `.github/workflows/dependency-review.yml:11`
- `.github/workflows/export-labels.yml:10`
- `.github/workflows/label-sync.yml:14`
- `.github/workflows/label-sync.yml:15`
- `.github/workflows/stale.yml:9`
- `.github/workflows/test.yml:11`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:37`
- `.github/workflows/test.yml:44`
- `.github/workflows/test.yml:46`
- `.github/workflows/test.yml:55`
- `.github/workflows/test.yml:57`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any of their jobs. Without explicit permissions, workflows inherit the repository's default token permissions (which may be broad), violating the principle of least privilege.

- export-labels.yml: no permissions block at all; the job uses secrets.GITHUB_TOKEN to access the GitHub API.
- label-sync.yml: no permissions block at all; the job checks out code and syncs labels.
- stale.yml: no permissions block at all; delegates to an external reusable workflow at EndBug/workflows@main.

Locations:

- `.github/workflows/export-labels.yml:1`
- `.github/workflows/label-sync.yml:1`
- `.github/workflows/stale.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 10 distinct action references to full commit SHAs across 7 workflow files (codeql-analysis.yml, dependabot-rebuild.yml, dependency-review.yml, export-labels.yml, label-sync.yml, stale.yml, test.yml). Original tag/branch names preserved as inline comments. Added minimal permissions blocks to the 3 workflows that lacked them: export-labels.yml (issues: read), label-sync.yml (issues: write), and stale.yml (issues: write, pull-requests: write).

