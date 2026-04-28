# agent-foundations — Repo Instructions

Audience: Claude (or any agent) editing this repo.

## What this repo is

A standalone framework for building durable AI agents:

- `conduct/` — model-agnostic behavioral modules (discipline, context, verification, delegation, tool-use, etc.)
- `engines/` — generic algorithmic primitives (Aho-Corasick, Beta-Bernoulli, Wald SPRT, Tarjan SCC, etc.)
- `taxonomy/` — failure-mode catalog with one doc per code
- `recipes/` — adoption guides per host (Claude Code, OpenAI Agents SDK, Cursor, generic system prompt)
- `docs/` — ADRs, architecture
- `anti-patterns.md`, `glossary.md` — cross-cutting references

## House rules for editing

1. **Model-agnostic.** Examples may name a vendor (Claude / GPT / Gemini), but no module assumes a specific harness, runtime, or product. If a paragraph only makes sense inside one product, it doesn't belong here.
2. **Cross-refs as relative paths.** Inside `conduct/`, sibling refs use `./X.md`. Across folders, use `../engines/X.md` style. Never hardcode absolute paths or org-specific roots.
3. **Granular commits.** One module = one commit. Conventional Commits format: `feat(conduct): add discipline.md`, `feat(engines): add trust-scoring.md`, `docs(recipes): add claude-code.md`, `chore: …`, `fix: …`.
4. **No vendor lock-in in code snippets.** Bash/JSON/Python examples illustrate; they don't depend on a specific tool's runtime.
5. **Marketing register lives in `README.md`.** Module bodies stay technical and tight. The hook/value-prop language goes in the top-level README only.

## Apply the conduct to yourself

Editing this repo means following [`conduct/discipline.md`](conduct/discipline.md), [`conduct/verification.md`](conduct/verification.md), and the rest. The framework is dogfooded — if you find yourself violating it while editing it, that's a signal the module needs to be sharper.

## License

MIT. Contributions are welcome under the same terms — see [LICENSE](LICENSE).
