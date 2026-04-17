# Exercise 8.2 — Cumulative: Hook-Injected Forced-Eval

**Time estimate:** 1.5–2 hours
**Dependencies:** Your S5.2 CLAUDE.md; the lab hook mechanics from 8.1
**Integrates:** S5 (CLAUDE.md forced-eval), S8 (hooks basics)

## Objective

Section 5's forced-eval lifted trigger rate from ~40% to ~84%. That's still 1-in-6 failures. Add a `UserPromptSubmit` hook that *injects* the forced-eval instruction every time, making it impossible to skip.

## Why cumulative

CLAUDE.md is hope. Hooks are certainty. You need both: CLAUDE.md sets the rules, the hook enforces them deterministically. This exercise makes the distinction real.

## Requirements

1. Write `submission/inject-forced-eval.sh` — a shell script that echoes the forced-eval instruction for Claude, naming *your* specific installed skills (parse from `.claude/skills/*/SKILL.md` if you want to be clever, or hardcode the list).
2. Install in `submission/settings.json` as a `UserPromptSubmit` hook.
3. **Compare** to your S5.2 CLAUDE.md forced-eval block:
   - Does the hook version add redundancy? (Probably yes.)
   - Keep both, or remove the CLAUDE.md version? Defend in reflection.
4. Test: run a 20-minute session with tasks that should trigger 2+ skills. Count how many times Claude's response opens with the skill-eval block.
5. Capture in `submission/session-log.md`.
6. **Iteration:** if any skill was missed, adjust the hook's injected prompt (add trigger phrases, examples, etc.). Re-test briefly. Document in `submission/iterations.md`.
7. Write `submission/reflection.md`:
   - Hook + CLAUDE.md: redundant, complementary, or one wins?
   - What's the failure mode of injecting forced-eval on *every* prompt? (Hint: chatty prompts, clarifications, etc.)
   - Is there a case for *conditional* injection (only on certain prompt types)?

## Submission

- `submission/inject-forced-eval.sh`
- `submission/settings.json`
- `submission/session-log.md`
- `submission/iterations.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 19 / 25 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Hook fires on every prompt** — verified | `[GATE]` | /5 | Gate: must actually inject. If invisible to Claude, fail. |
| **Injected prompt names your skills** — not generic | `[GATE]` | /5 | Gate: "use applicable skills" = fail. Must name specific skills. |
| **Real 20-min session** — honest log | `[GATE]` | /5 | Gate: synthetic = fail |
| **Redundancy analysis** — CLAUDE.md vs hook | — | /5 | Thoughtful position, not "both is better" |
| **Conditional-injection argument** — for or against | — | /5 | Names a specific failure mode of always-inject |

## What "good" looks like

Your hook reads `.claude/skills/*/SKILL.md` and assembles a list. Session log shows the eval-block fired on 18/20 prompts — missed 2 that were clarifications ("yes"/"go ahead"). Your iteration adds logic: skip injection if the user's message is < 15 characters. Reflection argues the hook replaces the CLAUDE.md forced-eval block entirely; you remove the duplicate and note that CLAUDE.md now only holds *what* to evaluate (skill list) while the hook enforces *that* you do evaluate.

## Integration with prior sections (targeted)

- **S5 — load-bearing.** Your CLAUDE.md forced-eval is the comparison baseline and possibly the thing you remove.
- **S8 — load-bearing.** Hook mechanics from 8.1.
