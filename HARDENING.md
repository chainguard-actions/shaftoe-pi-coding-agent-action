<!-- markdownlint-disable -->

# Hardening Report: shaftoe--pi-coding-agent-action/v2.19.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **shaftoe--pi-coding-agent-action/v2.19.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tag or branch refs instead of immutable 40-character commit SHAs. This exposes the workflows to supply-chain attacks if any of the referenced actions are compromised or their tags are moved. Failing references include: actions/checkout@v6, oven-sh/setup-bun@v2, codecov/codecov-action@v6, actions/setup-node@v6, shaftoe/update-bun-dependencies-action@v1, fallow-rs/fallow@v2, github/codeql-action/upload-sarif@v4, shaftoe/pi-coding-agent-action@develop, actions/upload-artifact@v7, actions/github-script@v9, cycjimmy/semantic-release-action@v6.

Locations:

- `.github/workflows/build_test_coverage.yml:20`
- `.github/workflows/build_test_coverage.yml:23`
- `.github/workflows/build_test_coverage.yml:33`
- `.github/workflows/daily-deps-update.yml:33`
- `.github/workflows/daily-deps-update.yml:35`
- `.github/workflows/daily-deps-update.yml:37`
- `.github/workflows/daily-deps-update.yml:41`
- `.github/workflows/fallow.yml:18`
- `.github/workflows/fallow.yml:21`
- `.github/workflows/fallow.yml:27`
- `.github/workflows/fallow.yml:33`
- `.github/workflows/package.yml:22`
- `.github/workflows/package.yml:25`
- `.github/workflows/pi.yml:29`
- `.github/workflows/pi.yml:32`
- `.github/workflows/pi.yml:35`
- `.github/workflows/pi.yml:39`
- `.github/workflows/pi.yml:55`
- `.github/workflows/pr.yml:17`
- `.github/workflows/pr.yml:20`
- `.github/workflows/pr.yml:23`
- `.github/workflows/pr.yml:27`
- `.github/workflows/pr.yml:55`
- `.github/workflows/promote-develop-to-v2.yml:24`
- `.github/workflows/promote-develop-to-v2.yml:62`
- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:25`
- `.github/workflows/release.yml:35`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all 11 unpinned action references across 7 workflow files (build_test_coverage.yml, daily-deps-update.yml, fallow.yml, package.yml, pi.yml, pr.yml, promote-develop-to-v2.yml, release.yml) to their full 40-character commit SHAs. Original tags/branches preserved as inline comments for readability. SHAs were resolved using lookup_action_sha for each action.

