<!-- markdownlint-disable -->

# Hardening Report: shaftoe--pi-coding-agent-action/v2.25.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **shaftoe--pi-coding-agent-action/v2.25.1** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved. Affected references include: actions/checkout@v7, oven-sh/setup-bun@v2, actions/setup-node@v6, codecov/codecov-action@v7, shaftoe/update-bun-dependencies-action@v1, actions/upload-artifact@v7, actions/download-artifact@v8, actions/github-script@v9, fallow-rs/fallow@v3, github/codeql-action/upload-sarif@v4, cycjimmy/semantic-release-action@v6, shaftoe/pi-coding-agent-action@develop.

Locations:

- `.github/workflows/build_test_coverage.yml:19`
- `.github/workflows/daily-deps-update.yml:38`
- `.github/workflows/develop.yml:49`
- `.github/workflows/fallow.yml:18`
- `.github/workflows/pi.yml:37`
- `.github/workflows/pr.yml:18`
- `.github/workflows/rebuild-dist.yml:28`
- `.github/workflows/release-promote.yml:43`
- `.github/workflows/release.yml:28`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation inside run: blocks. In the 'Extract changed package.json files' step, ${{ matrix.dir }} is interpolated directly into shell commands: `echo "No changes in ${{ matrix.dir }}"` and `SAFE_NAME=$(echo '${{ matrix.dir }}' | sed ...)`. In the 'Upload changed files' step, step outputs are interpolated directly: `SAFE_NAME="${{ steps.changes.outputs.safe-name }}"` and `tar cf "${ARCHIVE}" ${{ steps.changes.outputs.file-list }}`. These allow injection of arbitrary shell metacharacters.

Locations:

- `.github/workflows/daily-deps-update.yml:64`
- `.github/workflows/daily-deps-update.yml:78`
- `.github/workflows/daily-deps-update.yml:88`
- `.github/workflows/daily-deps-update.yml:90`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all unpinned action references across 9 workflow files by replacing mutable tags with full SHA commits (with tag comments for readability). Fixed script injection in daily-deps-update.yml by moving ${{ matrix.dir }} into MATRIX_DIR env var (used in 'Extract changed package.json files' step) and moving ${{ steps.changes.outputs.safe-name }} and ${{ steps.changes.outputs.file-list }} into SAFE_NAME and FILE_LIST env vars (used in 'Upload changed files' step). Pinned SHAs: actions/checkout@9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0, oven-sh/setup-bun@0c5077e51419868618aeaa5fe8019c62421857d6, actions/setup-node@48b55a011bda9f5d6aeb4c2d9c7362e8dae4041e, codecov/codecov-action@fb8b3582c8e4def4969c97caa2f19720cb33a72f, shaftoe/update-bun-dependencies-action@03fd49bd13a4b0ac5591ca4c7831bb551a2cf968, actions/upload-artifact@043fb46d1a93c77aae656e7c1c64a875d1fc6a0a, actions/download-artifact@3e5f45b2cfb9172054b4087a40e8e0b5a5461e7c, actions/github-script@3a2844b7e9c422d3c10d287c895573f7108da1b3, fallow-rs/fallow@c9326d9395c0b46f4077290473060b3e950b6e8c, github/codeql-action/upload-sarif@99df26d4f13ea111d4ec1a7dddef6063f76b97e9, cycjimmy/semantic-release-action@b12c8f6015dc215fe37bc154d4ad456dd3833c90, shaftoe/pi-coding-agent-action@ff672573b4f2404d7f8e2cf896f8c186988e83ae.

### Iteration 2

**Fixes applied:** github-env-injection, script-injection

**Notes:**

Fixed three findings in .github/workflows/daily-deps-update.yml:
1. (line 89) github-env-injection: Added `| tr -d '\n\r'` to the SAFE_NAME sed pipeline in 'Extract changed package.json files', and replaced `echo "safe-name=..."` with `printf 'safe-name=%s\n'` to properly sanitize before writing to GITHUB_OUTPUT.
2. (line 104) github-env-injection: In 'Upload changed files', added `SAFE_NAME_CLEAN=$(printf '%s' "${SAFE_NAME}" | tr -d '\n\r')` to strip newlines from the steps output value before writing `archive-name=...` to GITHUB_OUTPUT via printf.
3. (line 103) script-injection: Replaced unquoted `${FILE_LIST}` expansion in `tar cf "${ARCHIVE}" ${FILE_LIST}` with a bash array: `read -ra FILES_ARRAY <<< "${FILE_LIST}"` followed by `tar cf "${ARCHIVE}" "${FILES_ARRAY[@]}"`, preventing word-splitting and glob-expansion injection.

