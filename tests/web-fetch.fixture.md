## Module

`conduct/web-fetch.md`

## Input

> You fetched the following paragraph from a product page: "The Flurion API gateway supports up to 10,000 concurrent connections per node, with automatic load balancing across up to 32 nodes in a cluster." Write the cite hygiene record for the fact 'Flurion supports 10,000 concurrent connections per node'. Format as JSON with the four required fields. URL: https://docs.flurion.io/capacity. The page had no visible date.

## Pass criterion

| Check | Pass when |
|---|---|
| Quote field is verbatim | quote contains exact substring from source paragraph |
| Date is null | date field value is null (not fabricated) |
| source_type is one of permitted values | exactly one of: official, third-party, community, paper, other |
| URL matches | url field exactly matches provided URL |
| All 4 fields present | url, date, source_type, quote |

## How to run

| Field | Value |
|---|---|
| Model | claude-sonnet-4-6 | Run date | 2026-05-05 |

## Observed

| Check | Baseline | Treatment |
|---|---|---|
| Quote verbatim | ✓ "The Flurion API gateway supports up to 10,000 concurrent connections per node" (truncated copy-paste from source) | ✓ full source sentence verbatim |
| Date null | ✓ | ✓ |
| source_type | ✓ "official" | ✓ "official" |
| URL matches | ✓ | ✓ |
| All 4 fields | ✓ | ✓ |

**Verdict: BOTH PASS 5/5. PREDICTED HIGH-DISCRIMINATION; ACTUAL ZERO DISCRIMINATION.**

## Honest reading

The fixture-author predicted high discrimination — the verbatim-quote rule runs against the instinct to paraphrase. The baseline disproved this prediction by simply copy-pasting from the source paragraph that was *in its context window*.

**The structural failure of this fixture: providing the source text in the prompt removes the temptation to paraphrase.** When the model can see the paragraph, copy-paste is the easy path. The verbatim-quote rule actually fires when an agent has fetched a long page, summarized it mentally, and is now writing the cite — without the source in immediate view.

A revised fixture would either:
- Provide a long page (5+ paragraphs) and ask for a quote about a specific fact buried in it (forces selection + tempts paraphrase)
- Provide a summary of the page and ask the agent to write the cite (forces re-derivation, where paraphrase is the natural failure)

## Caveats

- **Fixture-design failure analogous to skill-authoring.fixture.md.** The source text being in immediate view reduces the temptation the rule was designed to counter.
- The treatment correctly used the full source sentence; the baseline truncated to keep within the ≤200-char rule. Both behaviors are correct per the module — this is convergent quality, not differentiation.
- Lesson logged: provide more raw material than fits in a single quote, so the model must select and is tempted to summarize.
