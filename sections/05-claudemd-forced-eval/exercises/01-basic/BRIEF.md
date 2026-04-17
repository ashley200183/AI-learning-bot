# Exercise 5.1 — CLAUDE.md with Forced-Eval

**Time estimate:** 1.5 hours
**Dependencies:** A project of yours with ≥2 installed skills

## Objective

Write a CLAUDE.md for a real project. Include a forced-eval block. Measure trigger reliability before vs after using the *same prompt sequence* on both sides for a clean A/B.

## Requirements

1. Pick a project with ≥2 installed skills.
2. Prepare 5 realistic prompts that *should* trigger skills. Save in `submission/test-prompts.md` (they'll be used identically before and after).
3. **Baseline:** in a clean session (no CLAUDE.md), run all 5 prompts. Record which skills fired in `submission/baseline.md`.
4. Write `submission/CLAUDE.md` with six sections:
   - Project context (stack, phase, constraints) — 3–5 lines
   - Forced-eval block
   - Always-apply conventions — 3–5 rules
   - Persona routing (if you have any from S4)
   - Compaction preservation rules
   - Anti-patterns — 2–3 things not to do
5. Install it at the project root. Start a fresh session. Run the **same 5 prompts** in the same order.
6. Record results in `submission/after.md`.
7. Write `submission/reflection.md`:
   - Trigger rate before vs after (same prompts, same skills — should be a clean comparison)
   - Any prompt where forced-eval caught a skill *you* forgot applied
   - Any unexpected side effects

## Submission

- `submission/test-prompts.md`
- `submission/baseline.md`
- `submission/CLAUDE.md`
- `submission/after.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **CLAUDE.md completeness** — all six sections, concise | — | /5 | |
| **Forced-eval block** — actually forces evaluation | `[GATE]` | /5 | Gate: passive "please consider skills" = fail. Must force a written decision. |
| **Same-prompts A/B** — identical prompts used both times | `[GATE]` | /5 | Gate: different prompts = no valid comparison. |
| **Trigger-rate delta measured** — clear numbers | — | /5 | |

## What "good" looks like

Same 5 prompts tested twice. Baseline: 2/5 skills fired. After: 5/5 with one skill Claude's written eval caught that *you* hadn't realized was relevant. Your reflection names the specific moment that skill's applicability surprised you.

## Integration with prior sections

None explicitly — this is foundational CLAUDE.md work using whatever skills you happen to have installed. The cumulative (5.2) integrates S2/S3/S4.
