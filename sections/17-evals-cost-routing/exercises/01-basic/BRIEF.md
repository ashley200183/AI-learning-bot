# Exercise 17.1 — Skill-Creator Eval

**Time estimate:** 1.5 hours
**Dependencies:** `skill-creator`; one of your skills from earlier sections

## Note on overlap with 2.1

Exercise 2.1 introduced skill-creator's eval briefly, on the *first* skill you built. This exercise goes deeper: pick a skill you've iterated on since (e.g., S2.2's second skill, or S7.1's handoff-writer) and run the eval rigorously with explicit plateau analysis.

## Objective

Run the skill-creator trigger-reliability eval on one of your mature skills. Capture every iteration. Identify the plateau and why.

## Requirements

1. Pick a skill. Best candidates: S2.2 second skill, S4.2 project-reviewer, S7.1 handoff-writer, S9.2's cred-scan skill if you built one, S10's heartbeat.
2. Run skill-creator's eval (rounds 1-5). Capture each round's:
   - Starting trigger rate
   - Proposed description changes
   - Ending trigger rate
   - Diff between description rounds
   → `submission/rounds.md`
3. Save final skill to `submission/final/`.
4. Write `submission/reflection.md`:
   - Which description changes moved the needle? Which didn't?
   - Did any round *decrease* the score? Why?
   - What's the plateau rate? What does it suggest about this skill's natural ceiling?
   - What's fundamentally un-capturable in a description for this skill?

## Submission

- `submission/rounds.md`
- `submission/final/`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **All 5 rounds captured** | `[GATE]` | /5 | Gate: any round fabricated or skipped = fail |
| **Diff analysis** — which changes mattered mechanically | `[GATE]` | /5 | Gate: "round 3 helped" without naming the edit = fail |
| **Final quality** — final rate ≥ round 0 | — | /5 | |
| **Plateau reasoning** — natural ceiling identified | — | /5 | Cites what descriptions fundamentally can't express |

## What "good" looks like

Round 0: 62%. Round 2 added "even when the user says" examples → 78%. Round 4 added anti-examples (when NOT to trigger) → 88%. Round 5 tried to boost further and regressed to 81% because tightening scope excluded legitimate cases. Plateau ~88% — remaining misses are prompts where the user asks something that's topically adjacent but semantically different. Reflection: *"No description can catch 'kinda like commits but for tags' — that needs the user to phrase it closer to the primary domain."*

## Integration with prior sections

None explicitly — uses one of your skills.
