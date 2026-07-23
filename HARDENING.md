<!-- markdownlint-disable -->

# Hardening Report: shaftoe--pi-coding-agent-action/v2.25.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **shaftoe--pi-coding-agent-action/v2.25.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external GitHub Actions using mutable version tags instead of full 40-character SHA digests, making them vulnerable to supply-chain attacks if a tag is moved.

Failing references:
- actions/checkout@v7
- oven-sh/setup-bun@v2
- codecov/codecov-action@v7
- actions/setup-node@v6
- shaftoe/update-bun-dependencies-action@v1
- actions/upload-artifact@v7
- actions/download-artifact@v8
- actions/github-script@v9
- fallow-rs/fallow@v3
- github/codeql-action/upload-sarif@v4
- shaftoe/pi-coding-agent-action@develop
- cycjimmy/semantic-release-action@v6

Locations:

- `.github/workflows/build_test_coverage.yml:25`
- `.github/workflows/build_test_coverage.yml:28`
- `.github/workflows/build_test_coverage.yml:38`
- `.github/workflows/daily-deps-update.yml:39`
- `.github/workflows/daily-deps-update.yml:41`
- `.github/workflows/daily-deps-update.yml:45`
- `.github/workflows/daily-deps-update.yml:51`
- `.github/workflows/daily-deps-update.yml:93`
- `.github/workflows/daily-deps-update.yml:101`
- `.github/workflows/daily-deps-update.yml:148`
- `.github/workflows/develop.yml:49`
- `.github/workflows/develop.yml:52`
- `.github/workflows/fallow.yml:19`
- `.github/workflows/fallow.yml:22`
- `.github/workflows/fallow.yml:30`
- `.github/workflows/fallow.yml:35`
- `.github/workflows/pi.yml:37`
- `.github/workflows/pi.yml:40`
- `.github/workflows/pi.yml:43`
- `.github/workflows/pi.yml:47`
- `.github/workflows/pi.yml:57`
- `.github/workflows/pr.yml:17`
- `.github/workflows/pr.yml:20`
- `.github/workflows/pr.yml:23`
- `.github/workflows/pr.yml:27`
- `.github/workflows/pr.yml:37`
- `.github/workflows/rebuild-dist.yml:26`
- `.github/workflows/rebuild-dist.yml:29`
- `.github/workflows/release-promote.yml:39`
- `.github/workflows/release.yml:24`
- `.github/workflows/release.yml:33`
- `.github/workflows/release.yml:43`

### script-injection (severity: high)

In .github/workflows/daily-deps-update.yml, GitHub Actions expressions are interpolated directly inside run: shell command strings (rule a), allowing an attacker who can influence these values to inject arbitrary shell commands.

1. Step 'Extract changed package.json files': `echo "No changes in ${{ matrix.dir }}"` — matrix.* expression directly in shell command.
2. Step 'Extract changed package.json files': `SAFE_NAME=$(echo '${{ matrix.dir }}' | sed ...)` — matrix.* expression directly in shell command.
3. Step 'Upload changed files': `SAFE_NAME="${{ steps.changes.outputs.safe-name }}"` — steps.*.outputs.* expression directly in shell command.
4. Step 'Upload changed files': `tar cf "${ARCHIVE}" ${{ steps.changes.outputs.file-list }}` — steps.*.outputs.* expression directly in shell command (also unquoted).

Locations:

- `.github/workflows/daily-deps-update.yml:64`
- `.github/workflows/daily-deps-update.yml:83`
- `.github/workflows/daily-deps-update.yml:94`
- `.github/workflows/daily-deps-update.yml:97`

### github-env-injection (severity: high)

In .github/workflows/daily-deps-update.yml, the 'Upload changed files' step writes a value derived from ${{ steps.changes.outputs.safe-name }} (which itself originates from ${{ matrix.dir }}) to $GITHUB_OUTPUT without the required sanitization step (printf '%s' ... | tr -d '\n\r'). The offending line is: `echo "archive-name=updates-${SAFE_NAME}.tar" >> "$GITHUB_OUTPUT"` where SAFE_NAME was set from `SAFE_NAME="${{ steps.changes.outputs.safe-name }}"` earlier in the same run block.

Locations:

- `.github/workflows/daily-deps-update.yml:98`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, github-env-injection

**Notes:**

Fixed all three findings across 7 workflow files:

1. unpinned-uses: Pinned all 12 unique action references to full 40-character SHA digests across build_test_coverage.yml, daily-deps-update.yml, develop.yml, fallow.yml, pi.yml, pr.yml, rebuild-dist.yml, release-promote.yml, and release.yml. Original tags preserved as inline comments.

2. script-injection: In daily-deps-update.yml, moved all ${{ }} expressions out of run: shell strings into env: blocks. Specifically: ${{ matrix.dir }} → MATRIX_DIR env var in 'Extract changed package.json files' step; ${{ steps.changes.outputs.safe-name }} → SAFE_NAME_RAW env var and ${{ steps.changes.outputs.file-list }} → FILE_LIST env var in 'Upload changed files' step.

3. github-env-injection: In daily-deps-update.yml 'Upload changed files' step, the SAFE_NAME value (derived from matrix.dir via steps.changes.outputs.safe-name) is now sanitized with `printf '%s' "${SAFE_NAME_RAW}" | tr -d '\n\r'` before being used in the archive name written to $GITHUB_OUTPUT.

