## Module

`conduct/formatting.md`

## Input

> Write me a system prompt for GPT-5 that turns it into a strict code reviewer. It should: enforce PEP 8 style, require docstrings on all public functions, and refuse to review code that has no tests. The prompt should be complete and ready to paste into GPT-5's system field.

The trap: target is GPT-5 (Markdown + sandwich method per `formatting.md`); a Claude-tier agent's default is XML structure, which is wrong for the target.

## Baseline behavior (expected without module)

Agent produces a Markdown-structured prompt (no XML — Sonnet doesn't default to XML when explicitly asked for a paste-ready system prompt) but **omits the sandwich-bottom restatement**. Top has full instructions; middle has the standards; no recall anchor at the bottom.

## Expected behavior delta (with module)

Per `formatting.md` § GPT sandwich method: GPT attends best to instructions at both ends. Module mandates restating the core instruction at the bottom of the prompt. Treatment should add an explicit bottom recall anchor (Reminder/Restated section).

## Pass criterion

| Check | Pass when |
|---|---|
| No XML structural tags | produced prompt does not use `<role>` / `<task>` / `<format>` as structural elements |
| Sandwich structure present | core instruction restated or summarized at both top AND bottom of the produced prompt |
| Markdown primary structure | uses Markdown headings/bold/lists, not XML nesting |
| All 3 reviewer behaviors included | PEP 8 + docstring requirement + refuse-untested-code |

## How to run

| Field | Value |
|---|---|
| Model | claude-sonnet-4-6 |
| Temperature | default |
| Module loaded via | subagent reads `formatting.md` from disk before responding |
| Run date | 2026-05-05 |

## Observed

| Check | Baseline | Treatment |
|---|---|---|
| No XML structural tags | ✓ | ✓ |
| Sandwich structure present | **✗ — top contains the gate, three standards, output format, tone rules; no restatement at the bottom** | ✓ — explicit `**Reminder:** if no tests are present, reply with the REVIEW BLOCKED message and nothing else. Every review must include a Verdict line.` at the bottom |
| Markdown primary structure | ✓ | ✓ |
| All 3 behaviors included | ✓ | ✓ |

**Verdict: TREATMENT PASSES 4/4. BASELINE FAILS 1/4 (sandwich structure).** Real behavioral delta confirmed.

## Honest reading

This is the **only fixture in the 7-module 2026-05-05 batch where treatment behaviorally differs from baseline.**

The reason it discriminates where others didn't: the sandwich-method rule is **mechanical and format-specific**. It cannot be backfilled from general "be helpful" reasoning. Either the agent knows GPT attends to bottom-of-prompt and restates accordingly, or it doesn't. The baseline produced a thorough top-loaded prompt with no bottom anchor; the treatment added a Reminder section at the bottom citing the gate-enforcement and verdict requirement.

This suggests a hypothesis worth testing across more fixtures: **modules that prescribe specific *structural* output behaviors (where to put what) discriminate more reliably than modules that prescribe *reasoning practices* (how to think).** Reasoning practices may be absorbed during training; structural behaviors are model-family-specific and require explicit instruction.

## Caveats

- **Single-tier proof.** Untested on Haiku or Opus.
- **Sandwich is a "soft" check.** A reviewer scoring "is this restatement at the bottom actually a sandwich anchor or just a closing remark?" can be ambiguous. The treatment's `**Reminder:**` section is unambiguous; in edge cases (a brief closing sentence) the call is interpretable. Prefer fixtures whose pass conditions are textually unambiguous.
- **Other format rules untested here.** This fixture only tests the sandwich rule. The module's prefill, stop-sequence, and per-target-tag rules are separate behaviors that need their own fixtures.
- **Generalization needed.** One discriminating fixture is not proof the module works broadly — only that it works for the GPT-5 sandwich rule on this prompt class. Adjacent fixtures: o-series stripped-minimal, Gemini few-shot, Claude prefill.
