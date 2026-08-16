<!-- markdownlint-disable -->

# Hardening Report: shaftoe--pi-coding-agent-action/v2.26.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **shaftoe--pi-coding-agent-action/v2.26.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across every workflow file use mutable version tags or branch names instead of immutable 40-character SHA commit hashes. This exposes the action to supply-chain attacks where a compromised upstream action tag could silently execute malicious code. Affected references include: `actions/checkout@v7`, `oven-sh/setup-bun@v2`, `actions/setup-node@v6`, `codecov/codecov-action@v7`, `shaftoe/update-bun-dependencies-action@v1`, `actions/upload-artifact@v7`, `actions/download-artifact@v8`, `actions/github-script@v9`, `fallow-rs/fallow@v3`, `github/codeql-action/upload-sarif@v4`, `shaftoe/pi-coding-agent-action@develop`, `cycjimmy/semantic-release-action@v6`.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/build_test_coverage.yml:22`
- `.github/workflows/comment.yml:1`
- `.github/workflows/daily-deps-update.yml:39`
- `.github/workflows/develop.yml:1`
- `.github/workflows/fallow.yml:18`
- `.github/workflows/pi.yml:40`
- `.github/workflows/pr.yml:18`
- `.github/workflows/rebuild-dist.yml:40`
- `.github/workflows/release-promote.yml:43`
- `.github/workflows/release.yml:28`

### script-injection (severity: high)

In `.github/workflows/daily-deps-update.yml`, GitHub Actions expressions are interpolated directly inside `run:` shell command strings (rule a), and one expansion is unquoted (rule b).

1. Line 64: `echo "No changes in ${{ matrix.dir }}"` — `${{ matrix.dir }}` is interpolated directly in a run: block.
2. Line 83: `SAFE_NAME=$(echo '${{ matrix.dir }}' | sed ...)` — `${{ matrix.dir }}` is interpolated directly in a run: block.
3. Line 94: `SAFE_NAME="${{ steps.changes.outputs.safe-name }}"` — `${{ steps.changes.outputs.safe-name }}` (a step output) is interpolated directly in a run: block.
4. Line 97: `tar cf "${ARCHIVE}" ${{ steps.changes.outputs.file-list }}` — `${{ steps.changes.outputs.file-list }}` is interpolated directly in a run: block AND is unquoted, allowing shell metacharacter injection from crafted filenames in the repository.

Locations:

- `.github/workflows/daily-deps-update.yml:64`
- `.github/workflows/daily-deps-update.yml:83`
- `.github/workflows/daily-deps-update.yml:94`
- `.github/workflows/daily-deps-update.yml:97`

### github-env-injection (severity: high)

In `.github/workflows/daily-deps-update.yml`, the 'Upload changed files' step (line 93–98) sets `SAFE_NAME` directly from `${{ steps.changes.outputs.safe-name }}` (a step output) and then writes a value derived from it to `$GITHUB_OUTPUT` without the required sanitization step (`printf '%s' ... | tr -d '\n\r'`). Specifically, line 94 sets `SAFE_NAME="${{ steps.changes.outputs.safe-name }}"` and line 98 writes `echo "archive-name=updates-${SAFE_NAME}.tar" >> "$GITHUB_OUTPUT"`. A newline embedded in the step output value could inject arbitrary key=value pairs into the GitHub output environment.

Locations:

- `.github/workflows/daily-deps-update.yml:94`
- `.github/workflows/daily-deps-update.yml:98`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, github-env-injection

**Notes:**

Fixed all three findings across 11 workflow files:

1. unpinned-uses: Pinned all 12 action references to immutable 40-char SHA hashes across build.yml, build_test_coverage.yml, comment.yml, daily-deps-update.yml, develop.yml, fallow.yml, pi.yml, pr.yml, rebuild-dist.yml, release-promote.yml, and release.yml. Original tags preserved as inline comments.

2. script-injection: In daily-deps-update.yml, moved all ${{ }} expressions out of run: shell blocks into env: blocks. matrix.dir → MATRIX_DIR env var; steps.changes.outputs.safe-name → CHANGES_SAFE_NAME env var; steps.changes.outputs.file-list → CHANGES_FILE_LIST env var. The file-list (a space-separated list) is tokenized safely using xargs+printf into a bash array to avoid shell metacharacter injection.

3. github-env-injection: In daily-deps-update.yml, all values derived from step outputs that are written to $GITHUB_OUTPUT are now sanitized with 'printf "%s" ... | tr -d "\n\r"' before writing, preventing newline injection attacks.

