# Exercise 4.1 — Compare Personas on the Same Code

**Time estimate:** 1 hour
**Dependencies:** `gstack` (installation instructions below)

## Objective

Run three different reviewers on the *same* 20 lines of code. Record what each catches that the others miss. Internalize: personas change *what Claude looks for*, not the code.

## Setup — install gstack

```bash
git clone https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
cd ~/.claude/skills/gstack
./setup
```

**Verify:** after setup completes, in a Claude Code session try `/review` — you should see the review persona activate. If `/review` isn't recognized, the setup didn't complete. Check the setup script output for errors. On macOS you may need to run `./setup` under `zsh` if your default shell changed.

## Requirements

1. Pick or write 20 lines of code with at least **one subtle issue**. Save as `submission/code-under-review.<ext>`. (Example: a function with a hidden race condition, a poor-naming collision, an off-by-one in pagination, an input-validation gap.)
2. Run three reviews. Between runs, use `/clear` to reset context — you want each review naive.
   - Default Claude (no persona): *"Review this code."*
   - `/review`
   - `/qa`
3. Save each in `submission/reviews/default.md`, `reviews/review.md`, `reviews/qa.md`.
4. Write `submission/comparison.md`:
   - What did each catch that the others missed? (Attribute specific findings.)
   - Where did they agree?
   - Which would you use for code about to ship? Which for code early in development? Why?

## Submission

- `submission/code-under-review.<ext>`
- `submission/reviews/default.md`
- `submission/reviews/review.md`
- `submission/reviews/qa.md`
- `submission/comparison.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Code has real issues** — not trivial | — | /5 | At least one non-obvious issue |
| **Three reviews captured cleanly** — naive runs | `[GATE]` | /5 | Gate: if you ran without `/clear` between, findings contaminate. Re-run. |
| **Comparison specificity** — findings attributed to specific personas | `[GATE]` | /5 | Gate: if findings aren't attributed (just listed), fail. |
| **When-to-use judgment** — context-mapped recommendations | — | /5 | Personas mapped to situations with reasoning from what you saw |

## What "good" looks like

Your comparison shows `/review` flagged naming and readability, `/qa` found a race condition by asking "what if two requests hit this simultaneously?", default Claude missed both. You note `/qa`'s critical move was adversarial framing — the code under review didn't change; the lens did.

## Integration with prior sections

**Section 3 — context.** `/review` and `/qa` are commands implemented as persona-loading. You're seeing the Section 3 mechanic applied to the Section 4 concept.
