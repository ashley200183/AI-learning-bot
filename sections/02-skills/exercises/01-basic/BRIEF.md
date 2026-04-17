# Exercise 2.1 — Build, Eval, and Iterate Your First Skill

**Time estimate:** 1.5–2 hours
**Dependencies:** Claude Code CLI; `skill-creator` plugin

## Objective

Build a commit-message skill using skill-creator. Run its trigger-reliability eval. Iterate the description until you're consistently above 80%.

## Setup — install skill-creator

```
/plugin install skill-creator@anthropic-agent-skills
```

**Verify:** ask Claude *"Use skill-creator to help me build a test skill."* You should see it enter the skill-building flow with questions.

## Requirements

1. Say: *"Use the skill-creator to help me build a skill for writing commit messages."*
2. Answer its questions. Let it generate the initial `SKILL.md`.
3. Copy the generated skill to `submission/commit-style/` (keep folder structure).
4. **Run skill-creator's built-in eval** on the skill. It will:
   - Generate test queries from the description
   - Run each 3x to get a reliable trigger rate
   - Propose description improvements
   - Iterate up to 5 times
5. Capture each round in `submission/eval-rounds.md`:
   - Round 0 (initial) trigger rate
   - For each proposed change: what changed and the new rate
6. Write `submission/test-prompts.md`: 5 of your own prompts you'd actually type when committing. Mix obvious ones with edge cases ("commit this", "push it up", "save progress"). Run each and note triggering.
7. Write `submission/reflection.md`:
   - Which description changes moved the needle? Which didn't?
   - Did any round *decrease* the score? Why?
   - How does skill-creator's test set compare to your 5 prompts?

## Submission

- `submission/commit-style/` — final skill
- `submission/eval-rounds.md`
- `submission/test-prompts.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 20 / 25 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Skill structure** — valid frontmatter, correct folder shape | `[GATE]` | /5 | Gate: skill must actually be loadable. Invalid frontmatter = fail. |
| **Description quality** — trigger phrases, action verbs, escape clauses | — | /5 | Compare to the strong example in Phase 2 of the source plan |
| **skill-creator eval run** — all rounds captured honestly | `[GATE]` | /5 | Gate: eval must actually have been run. Fabricated numbers = fail. |
| **Trigger reliability** — final trigger rate on your 5 prompts | `[GATE]` | /5 | Gate: ≥ 3/5 prompts must trigger. <3 = fail. Full credit at 5/5. |
| **Iteration reflection** — named what changed and why | — | /5 | Specific descriptions, not "I improved it" |

## What "good" looks like

Your `eval-rounds.md` shows round 0 at 60%, round 2 adding "even when the user just says 'commit this'" boosting to 85%, round 4 adding explicit handling of "push" and "save" reaching 92%. Your 5 personal prompts fire on 5/5. Your reflection names exactly which phrase additions drove each jump.

## Integration with prior sections

None — this is your first built skill. Section 1's "observe triggering" exercise is the motivation; this is the build.
