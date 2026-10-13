# End-to-end test: `copperhead create` (brief → clean full run) + findings

## What

Adds an end-to-end test harness and findings report for the `copperhead create`
flow, exercising the full path from a brief file through to a clean completed
run. The harness drives the real CLI binary (no mocks), asserts on exit codes,
generated artifacts, and idempotent re-runs.

## Why

Issue #66 requests an end-to-end test of `copperhead create` with a findings
report. There was no test covering the brief → clean full run path, so
regressions in argument parsing, file emission, or run completion could ship
unnoticed. This closes that gap and documents observed behavior.

## Testing

- `go test ./...` — all existing packages pass.
- `bash scripts/e2e_create.sh` — runs the full brief → clean run flow against a
  temporary workspace and asserts:
  - exit code 0 on a valid brief,
  - expected output artifacts exist and are non-empty,
  - a second run is idempotent (no duplicate/corrupt output).

## Invariant checklist

- [ ] No changes to production behavior; test-only additions.
- [ ] Harness uses a temp dir and cleans up after itself.
- [ ] Findings report is factual and reproducible.
- [ ] Existing tests continue to pass.

## Findings report

See `docs/e2e-findings.md` (this file) for the observed behavior, edge cases
(hyphenated brief paths, pre-existing output dir), and two low-severity
robustness notes with suggested follow-ups.
