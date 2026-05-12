# Changelog

## Unreleased — 2026-05-12

### Fixed

- **foundations-missing failure mode now fails loud.** Per s2.0 recommendation (lifecycle-research arc). Cloning a sibling plugin into a fresh directory without `enchanter-foundations` alongside used to leave `@-imports` silently missing (Claude Code's `@`-loader fails soft). Now:
  - `scripts/bootstrap.sh` auto-clones `enchanter-foundations` from `ENCHANTER_FOUNDATIONS_REPO` (default: github.com/enchanter-ai/enchanter-foundations) if not alongside.
  - `scripts/hooks/sessionstart-foundations-drift.sh` fires on every Claude Code session start; exits non-zero with `"foundations sibling missing — run ./scripts/bootstrap.sh"` if absent.
  - `.github/workflows/foundations-verify.yml` runs `./scripts/bootstrap.sh --verify` on every push to main; CI fails on any drift or missing-foundations state.
- **Drift detection.** `.foundations-lock` pins `foundations_commit` + per-package `tag_commit` + per-conduct-file SHA1. `bootstrap.sh --verify` fails loud with a verbatim one-line remediation on any divergence (corruption, foundations HEAD move, missing file).
- **Verified before/after.** Sandbox-tested with a fresh clone of `crow` in isolation; before: silent fail; after: loud fail with named remediation. Sandbox report at the recommendation workspace.

### Added (Layer 1–3)

- `packages/orchestration/templates/bootstrap.sh` + `.ps1` (Bash + PowerShell) — resolve / verify modes.
- `packages/orchestration/templates/sessionstart-foundations-drift.sh` — SessionStart hook template.
- `packages/orchestration/templates/foundations-verify.yml` — GitHub Actions workflow template.
- `packages/orchestration/docs/foundations-lock-spec.md` — lock-file schema specification.

### Documented (Layer 4 — not shipped)

- `docs/future/org-mirror.md` — Cargo source-replacement pattern-precedent + 3 trigger conditions. Built only when triggers fire.


