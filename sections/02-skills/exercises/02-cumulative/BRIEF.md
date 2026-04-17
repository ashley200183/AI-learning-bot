# Exercise 2.2 — Cumulative: Build a Second Skill From Your Anti-patterns

**Time estimate:** 1–1.5 hours
**Dependencies:** Section 2.1 completed (your commit-style skill); `skill-creator`
**Integrates:** Section 1 (mental models — picking skill-worthy tasks)

## Objective

Revisit your Section 1 anti-patterns list. Was anything on it actually skill-worthy but you dismissed it too quickly? Build a skill for one such task. Apply everything from Section 2.1 without skill-creator holding your hand this time.

## Why cumulative

Section 1 asked you to notice when agents are *wrong*. This exercise asks: within the tasks you thought weren't worth an agent, are any worth a *skill*? A skill is lighter — it's a cheat sheet, not a team member. The line between "not worth an agent" and "absolutely worth a skill" is the Section 1 concept in your fingers.

## Requirements

1. Re-read your `submission/anti-patterns.md` from Exercise 1.1.
2. Pick one that on reflection *is* skill-worthy: something you do repeatedly, the procedure is repeatable, but you were right that "an agent" would be overkill. Common candidates: test-writing conventions, PR description format, release notes style, API endpoint patterns.
3. **Hand-write the first draft** at `submission/<skill-name>/SKILL.md` — do NOT use skill-creator for this round. Apply what you learned from Exercise 2.1's iterations directly.
4. Write 5 test prompts in `submission/test-prompts.md` and run them.
5. If trigger rate < 4/5, iterate the description based on what you learned in 2.1. Capture each iteration in `submission/iterations.md`.
6. Now run `skill-creator`'s eval on your hand-written skill. Compare what it proposes to what you already did. Capture in `submission/skill-creator-comparison.md`.
7. Write `submission/reflection.md`:
   - Why was this task skill-worthy despite being on your anti-patterns list?
   - Where did your hand-written description match what skill-creator would have produced? Where did it differ?
   - If you had to write a skill with no tools at all (just your head), what are the two or three description rules you now carry?

## Submission

- `submission/<skill-name>/SKILL.md`
- `submission/test-prompts.md`
- `submission/iterations.md`
- `submission/skill-creator-comparison.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 23 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Skill picks a real task** — drawn from your anti-patterns list, genuinely repeats in your work | `[GATE]` | /5 | Gate: contrived tasks fail. Must reference the anti-pattern you're converting. |
| **Hand-written first draft** — you wrote it, not skill-creator | `[GATE]` | /5 | Gate: if the initial draft is obviously generated, fail. Iterations afterward are fine. |
| **Description quality** — applies 2.1 lessons (trigger phrases, escape clauses) | — | /5 | |
| **Trigger reliability** — final rate on your 5 prompts | `[GATE]` | /5 | Gate: ≥ 3/5 trigger. |
| **skill-creator comparison** — honest delta analysis | — | /5 | Where you matched, where you missed, where you disagreed |
| **Section 1 integration** — converts an anti-pattern with defensible reasoning | — | /5 | Not just "I changed my mind." Names what about the task makes it skill-worthy after all. |

## What "good" looks like

Your reflection explains: *"I listed 'writing unit tests' as something I'd do manually because an agent felt heavy. But a skill captures the conventions I keep forgetting (file placement, the one mock helper we use, the describe/it pattern). That's not an agent — it's a sticky note. Skill-worthy."* Your skill-creator comparison shows you independently arrived at 80% of what it would have suggested.

## Integration with prior sections

**Section 1 — targeted.** You're applying the mental model distinction (skill vs agent) to reclassify your own anti-patterns. The grader will verify your anti-patterns.md is actually referenced, not just name-checked.
