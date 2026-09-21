<!-- markdownlint-disable -->

# Hardening Report: shaftoe--pi-coding-agent-action/v2.27.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **shaftoe--pi-coding-agent-action/v2.27.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of pinned full-length commit SHAs. This exposes the pipeline to supply-chain attacks where a compromised or malicious tag update could execute arbitrary code. Affected references include: actions/checkout@v7, pnpm/action-setup@v6, actions/setup-node@v7, codecov/codecov-action@v7, fallow-rs/fallow@v3, github/codeql-action/upload-sarif@v4.37.3, shaftoe/pi-coding-agent-action@develop (branch reference — especially risky), actions/upload-artifact@v7, and cycjimmy/semantic-release-action@v6. Each should be pinned to a full 40-character commit SHA (e.g. uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v7).

Locations:

- `.github/workflows/build_test_coverage.yml:19`
- `.github/workflows/build_test_coverage.yml:23`
- `.github/workflows/build_test_coverage.yml:27`
- `.github/workflows/build_test_coverage.yml:40`
- `.github/workflows/develop.yml:55`
- `.github/workflows/develop.yml:59`
- `.github/workflows/develop.yml:63`
- `.github/workflows/fallow.yml:18`
- `.github/workflows/fallow.yml:22`
- `.github/workflows/fallow.yml:26`
- `.github/workflows/fallow.yml:34`
- `.github/workflows/fallow.yml:39`
- `.github/workflows/pi.yml:35`
- `.github/workflows/pi.yml:39`
- `.github/workflows/pi.yml:43`
- `.github/workflows/pi.yml:60`
- `.github/workflows/pr.yml:18`
- `.github/workflows/pr.yml:22`
- `.github/workflows/pr.yml:26`
- `.github/workflows/pr.yml:55`
- `.github/workflows/rebuild-dist.yml:30`
- `.github/workflows/rebuild-dist.yml:34`
- `.github/workflows/rebuild-dist.yml:38`
- `.github/workflows/release-promote.yml:43`
- `.github/workflows/release.yml:24`
- `.github/workflows/release.yml:34`
- `.github/workflows/release.yml:38`
- `.github/workflows/release.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all 28 unpinned action references across 7 workflow files:
- actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1 # v7
- pnpm/action-setup@v6 → @0977fd99725f1db4007ccb2928dbb4e90d06cc86 # v6
- actions/setup-node@v7 → @820762786026740c76f36085b0efc47a31fe5020 # v7
- codecov/codecov-action@v7 → @fb8b3582c8e4def4969c97caa2f19720cb33a72f # v7
- fallow-rs/fallow@v3 → @ecf5a314fd3e10974acb4f5a7f867c433030522d # v3
- github/codeql-action/upload-sarif@v4.37.3 → @e4fba868fa4b1b91e1fdab776edc8cfbe6e9fb81 # v4.37.3
- shaftoe/pi-coding-agent-action@develop → @ccd45ba801eb4c4c503a1c7ed463075e18d9dce6 # develop
- actions/upload-artifact@v7 → @043fb46d1a93c77aae656e7c1c64a875d1fc6a0a # v7
- cycjimmy/semantic-release-action@v6 → @b12c8f6015dc215fe37bc154d4ad456dd3833c90 # v6

Files modified: build_test_coverage.yml, develop.yml, fallow.yml, pi.yml, pr.yml, rebuild-dist.yml, release-promote.yml, release.yml. The release.yml file was fully rewritten after it got corrupted during sequential edits.

