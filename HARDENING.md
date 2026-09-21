<!-- markdownlint-disable -->

# Hardening Report: shaftoe--pi-coding-agent-action/v2.28.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **shaftoe--pi-coding-agent-action/v2.28.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use mutable version tags instead of pinned full-length SHA commit hashes for their `uses:` references. This exposes the pipeline to supply-chain attacks where a tag is moved to point at malicious code.

Failing references:
- .github/workflows/build.yml: (delegates to build_test_coverage.yml)
- .github/workflows/build_test_coverage.yml: `actions/checkout@v7`, `pnpm/action-setup@v6`, `actions/setup-node@v7`, `codecov/codecov-action@v7`
- .github/workflows/develop.yml: `actions/checkout@v7`, `pnpm/action-setup@v6`, `actions/setup-node@v7`
- .github/workflows/fallow.yml: `actions/checkout@v7`, `pnpm/action-setup@v6`, `actions/setup-node@v7`, `fallow-rs/fallow@v3`, `github/codeql-action/upload-sarif@v4.37.9`
- .github/workflows/pi.yml: `actions/checkout@v7`, `actions/setup-node@v7`, `shaftoe/pi-coding-agent-action@develop`, `actions/upload-artifact@v7`
- .github/workflows/pr.yml: `actions/checkout@v7`, `actions/setup-node@v7`, `shaftoe/pi-coding-agent-action@develop`, `actions/upload-artifact@v7`
- .github/workflows/rebuild-dist.yml: `actions/checkout@v7`, `pnpm/action-setup@v6`, `actions/setup-node@v7`
- .github/workflows/release-promote.yml: `actions/checkout@v7`
- .github/workflows/release.yml: `actions/checkout@v7`, `pnpm/action-setup@v6`, `actions/setup-node@v7`, `cycjimmy/semantic-release-action@v6`

All references should be pinned to a full 40-character hex SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`. Notably, `shaftoe/pi-coding-agent-action@develop` uses a branch name, which is especially dangerous as it can be updated at any time.

Locations:

- `.github/workflows/build_test_coverage.yml:22`
- `.github/workflows/build_test_coverage.yml:25`
- `.github/workflows/build_test_coverage.yml:28`
- `.github/workflows/build_test_coverage.yml:38`
- `.github/workflows/develop.yml:43`
- `.github/workflows/develop.yml:48`
- `.github/workflows/develop.yml:51`
- `.github/workflows/fallow.yml:18`
- `.github/workflows/fallow.yml:22`
- `.github/workflows/fallow.yml:25`
- `.github/workflows/fallow.yml:33`
- `.github/workflows/fallow.yml:38`
- `.github/workflows/pi.yml:35`
- `.github/workflows/pi.yml:38`
- `.github/workflows/pi.yml:43`
- `.github/workflows/pi.yml:58`
- `.github/workflows/pr.yml:18`
- `.github/workflows/pr.yml:22`
- `.github/workflows/pr.yml:27`
- `.github/workflows/pr.yml:52`
- `.github/workflows/rebuild-dist.yml:28`
- `.github/workflows/rebuild-dist.yml:31`
- `.github/workflows/rebuild-dist.yml:34`
- `.github/workflows/release-promote.yml:38`
- `.github/workflows/release.yml:25`
- `.github/workflows/release.yml:36`
- `.github/workflows/release.yml:39`
- `.github/workflows/release.yml:47`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all unpinned `uses:` references across 8 workflow files to full 40-character SHA hashes:
- actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1 # v7
- pnpm/action-setup@v6 → @0977fd99725f1db4007ccb2928dbb4e90d06cc86 # v6
- actions/setup-node@v7 → @820762786026740c76f36085b0efc47a31fe5020 # v7
- codecov/codecov-action@v7 → @fb8b3582c8e4def4969c97caa2f19720cb33a72f # v7
- fallow-rs/fallow@v3 → @c55ba1ba3c98c6e4ad48f0aed0e0f256c256d959 # v3
- github/codeql-action/upload-sarif@v4.37.9 → @cdf488f595d80d6e07e03d4674febd5ab45fa938 # v4.37.9
- shaftoe/pi-coding-agent-action@develop → @1f0be2391705316c12e0f504eab2e39c74ec2da8 # develop
- actions/upload-artifact@v7 → @043fb46d1a93c77aae656e7c1c64a875d1fc6a0a # v7
- cycjimmy/semantic-release-action@v6 → @b12c8f6015dc215fe37bc154d4ad456dd3833c90 # v6

Files modified: build_test_coverage.yml, develop.yml, fallow.yml (rewritten), pi.yml, pr.yml, rebuild-dist.yml, release-promote.yml, release.yml (rewritten). fallow.yml and release.yml were fully rewritten to fix corruption from concurrent edits.

