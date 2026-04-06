# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A GitHub Action (`cgrindel/gha_set_up_bazel@v2`) that wraps `bazel-contrib/setup-bazel` with two
fixes: removing the `startup --output_base` override (prevents lock contention in integration tests)
and enabling `--config=ci` in `~/.bazelrc`.

The action is defined entirely in `action.yml` (composite action) — there is no build step or
compiled code.

## Testing

Tests run in CI only (GitHub Actions). The CI workflow (`.github/workflows/ci.yml`) exercises the
action with `uses: ./` and verifies behavior by inspecting `~/.bazelrc`. There are no local test
commands.

The `ci/` directory contains a minimal Bazel workspace used by CI to validate that Bazel works after
the action runs:

```bash
cd ci && bazel test //...
```

## Release

Merges to `main` trigger `.github/workflows/release.yml`, which auto-bumps the patch version tag
and moves the `v2` major version tag forward.

## Key Files

- `action.yml` — the entire action implementation
- `.github/workflows/ci.yml` — CI tests (two scenarios: default settings, CI config disabled)
- `.github/workflows/release.yml` — automated release on merge to main
- `ci/` — test Bazel workspace with `ci.bazelrc` and `shared.bazelrc`
