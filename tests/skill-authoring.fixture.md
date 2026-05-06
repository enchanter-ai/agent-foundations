## Module

`conduct/skill-authoring.md`

## Input

> Write the YAML frontmatter block for a new skill called `deduplicate-claims`. It takes a `claims.json` file, merges near-duplicate claims, and returns a deduplicated list. It should fire when a user asks to clean up research findings, collapse duplicate claims, or reduce redundancy in a claims file. It only needs to read and write one file.

## Pass criterion

| Check | Pass when |
|---|---|
| "Use when" trigger clause | description contains a clause naming when to invoke the skill |
| Third person description | no first-person ("I") or second-person ("you") pronouns in description |
| Minimal tool whitelist | tools field contains at most [Read, Write] |
| All 4 required fields present | name, description, model, tools |
| Description length 100-1024 chars | within bounds |

## How to run

| Field | Value |
|---|---|
| Model | claude-sonnet-4-6 | Run date | 2026-05-05 |

## Observed

| Check | Baseline | Treatment |
|---|---|---|
| "Use when" trigger clause | ✓ ("Use when the user asks to clean up research findings...") | ✓ ("Use when: the user asks...") |
| Third person | ✓ | ✓ |
| Minimal tool whitelist | ✓ `tools: [Read, Write]` | ✓ `tools: [Read, Write]` |
| All 4 fields | ✓ name, description, model: sonnet, tools | ✓ name, description, model: haiku, tools |
| Description length | ✓ ~440 chars | ✓ ~530 chars |

**Verdict: BOTH PASS 5/5. NO BEHAVIORAL DELTA on this model.**

## Honest reading

The fixture failed to discriminate because the **input prompt explicitly prescribed the trigger conditions** ("It should fire when..."). The agent simply transcribed those into a "Use when" clause. The minimal tool whitelist was also cued by the prompt ("It only needs to read and write one file"). Both pass criteria were leaked by the prompt design.

The treatment differs only in:
- `model: haiku` (treatment correctly identified the task as low-tier transformation; baseline picked sonnet)
- Slightly more explicit "Do not use" clause for adjacent skills

These are quality differences, not behavioral discriminators on the pass criteria.

## Caveats

- **Fixture-design failure analogous to discipline.fixture.md Run 1.** The prompt cued the answer. A revised fixture should ask the model to *infer* trigger conditions from a task description that doesn't list them explicitly, e.g., "Write the SKILL.md for a skill that takes claims.json and produces a deduplicated list."
- The model-tier choice (haiku vs sonnet) is a real treatment-side improvement worth noting, but it isn't in the pass criteria.
- Lesson logged: prompts that prescribe the answer cannot test for the answer.
