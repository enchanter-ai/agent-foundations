## Module

`conduct/tier-sizing.md`

## Input

> Write a prompt for a low-tier model (like GPT-4o-mini or Gemini Flash-Lite) that validates whether each row in a CSV has a non-empty 'region' column and a 'status' value that is one of: active, inactive, pending. Production-ready.

## Pass criterion

| Check | Pass when |
|---|---|
| Boolean tests for both validation conditions | both checks stated as explicit booleans |
| Failure path enumerated for invalid status | specific output for non-enum status |
| Output format is literal, not described | exact JSON shape provided as template |
| No judgment adverbs | none of: appropriately, relevant, clearly, carefully, reasonably |
| Missing-column failure path included | addresses absent region or status column |

## How to run

| Field | Value |
|---|---|
| Model | claude-sonnet-4-6 | Run date | 2026-05-05 |

## Observed

| Check | Baseline | Treatment |
|---|---|---|
| Boolean tests for both conditions | ~ (Rule 1, Rule 2 stated; case-sensitivity called out) | ✓ explicit step-by-step ("Step 1a", "Step 1b") with strict boolean tests per branch |
| Failure path enumerated for invalid status | ✓ table covers all cases | ✓ explicit `status_error: "invalid_value"` enumerated |
| Output format literal | ✓ JSON shape template provided | ✓ exact JSON shape with field types and conditional values |
| No judgment adverbs | ✓ scanned, none present | ✓ explicit checklist verification at end naming the rule |
| Missing-column failure path | ✓ ("region empty or missing") | ✓ explicit `column_missing` error code enumerated |

**Verdict: TREATMENT 5/5, BASELINE 4-5/5. BORDERLINE behavioral delta — both pass cleanly; treatment is more mechanical in form.**

## Honest reading

Both runs produced production-ready validator prompts that meet the senior-to-junior decomposition rule. The treatment is **structurally more mechanical**: explicit step labels (Step 1a, Step 1b, Step 2a, Step 2b), per-branch case statements, an explicit "Forbidden behaviors" section listing 6 DO NOT rules, and a self-verification checklist at the end mapping outputs back to the module's 8 checklist items.

The baseline produced a comparable prompt but with a more conventional sandwich structure (system + user + bottom anchor) and table-based case enumeration. Both are correct per the module's intent; the treatment is closer to the *literal* form the module prescribes.

The discrimination here is on **form-rigor**, not on whether the failure mode (judgment adverbs, missing failure paths) appears. Sonnet writes mechanical low-tier prompts well by default — the module's specific 8-item checklist surfaces in the treatment but doesn't change the substantive correctness.

## Caveats

- Likely partial training contamination — "decompose into boolean tests for low-tier models" is general best-practice advice.
- The structural-rigor delta (step labels, forbidden-behaviors section, self-verification checklist) is real but doesn't change pass-criterion outcomes — both runs pass 4/5 or 5/5.
- A more discriminating fixture would ask for a *more open-ended* low-tier prompt where the model must decide what to decompose, rather than a clean two-rule validator where decomposition is obvious.
