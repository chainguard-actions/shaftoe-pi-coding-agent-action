<!-- markdownlint-disable -->

# Hardening Report: shaftoe--pi-coding-agent-action/v2.27.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **shaftoe--pi-coding-agent-action/v2.27.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external GitHub Actions using mutable tags, version strings, or branch names instead of pinned 40-character commit SHAs. This exposes the pipeline to supply-chain attacks where a compromised or updated action tag could execute malicious code.

Failing references in build_test_coverage.yml: actions/checkout@v7, pnpm/action-setup@v6, actions/setup-node@v7, codecov/codecov-action@v7.

Failing references in fallow.yml: actions/checkout@v7, pnpm/action-setup@v6, actions/setup-node@v7, fallow-rs/fallow@v3, github/codeql-action/upload-sarif@v4.37.7.

Failing references in pi.yml: actions/checkout@v7, actions/setup-node@v7, shaftoe/pi-coding-agent-action@develop (branch ref — especially dangerous), actions/upload-artifact@v7.

Failing references in pr.yml: actions/checkout@v7, actions/setup-node@v7, shaftoe/pi-coding-agent-action@develop (branch ref — especially dangerous), actions/upload-artifact@v7.

Failing references in develop.yml: actions/checkout@v7, pnpm/action-setup@v6, actions/setup-node@v7.

Failing references in rebuild-dist.yml: actions/checkout@v7, pnpm/action-setup@v6, actions/setup-node@v7.

Failing references in release-promote.yml: actions/checkout@v7.

Failing references in release.yml: actions/checkout@v7, pnpm/action-setup@v6, actions/setup-node@v7, cycjimmy/semantic-release-action@v6.

All should be pinned to full 40-character commit SHAs (e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4).

Locations:

- `.github/workflows/build_test_coverage.yml:24`
- `.github/workflows/build_test_coverage.yml:27`
- `.github/workflows/build_test_coverage.yml:30`
- `.github/workflows/build_test_coverage.yml:43`
- `.github/workflows/fallow.yml:19`
- `.github/workflows/fallow.yml:23`
- `.github/workflows/fallow.yml:26`
- `.github/workflows/fallow.yml:36`
- `.github/workflows/fallow.yml:42`
- `.github/workflows/pi.yml:17`
- `.github/workflows/pi.yml:21`
- `.github/workflows/pi.yml:25`
- `.github/workflows/pi.yml:47`
- `.github/workflows/pr.yml:15`
- `.github/workflows/pr.yml:19`
- `.github/workflows/pr.yml:23`
- `.github/workflows/pr.yml:46`
- `.github/workflows/develop.yml:52`
- `.github/workflows/develop.yml:56`
- `.github/workflows/develop.yml:59`
- `.github/workflows/rebuild-dist.yml:30`
- `.github/workflows/rebuild-dist.yml:34`
- `.github/workflows/rebuild-dist.yml:38`
- `.github/workflows/release-promote.yml:42`
- `.github/workflows/release.yml:33`
- `.github/workflows/release.yml:39`
- `.github/workflows/release.yml:43`
- `.github/workflows/release.yml:52`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all unpinned GitHub Actions references across 8 workflow files:

- actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1 # v7
- pnpm/action-setup@v6 → @0977fd99725f1db4007ccb2928dbb4e90d06cc86 # v6
- actions/setup-node@v7 → @820762786026740c76f36085b0efc47a31fe5020 # v7
- codecov/codecov-action@v7 → @fb8b3582c8e4def4969c97caa2f19720cb33a72f # v7
- fallow-rs/fallow@v3 → @a87665c0837d60020b8d8dce1bd4920139d5fb9c # v3
- github/codeql-action/upload-sarif@v4.37.7 → @ff2f1c621b7f889edc0d3c761ac2e6a3f8cdb0dd # v4.37.7
- shaftoe/pi-coding-agent-action@develop → @c1e0b11c0b667f8e8fe9d0df8810c0745bfff59e # develop
- actions/upload-artifact@v7 → @043fb46d1a93c77aae656e7c1c64a875d1fc6a0a # v7
- cycjimmy/semantic-release-action@v6 → @b12c8f6015dc215fe37bc154d4ad456dd3833c90 # v6

Files modified: build_test_coverage.yml, fallow.yml, pi.yml, pr.yml, develop.yml, rebuild-dist.yml, release-promote.yml, release.yml. Two files (pi.yml and pr.yml) required full rewrites due to edit corruption from overlapping replacements.

