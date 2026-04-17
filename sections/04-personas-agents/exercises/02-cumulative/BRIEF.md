# Exercise 4.2 — Cumulative: Persona + Subagent + Skill + Command

**Time estimate:** 2–3 hours
**Dependencies:** Your S2.1 commit-style skill; your S3.1 command; skill-creator
**Integrates:** Section 2 (skill), Section 3 (command), Section 4 (persona + subagent)

## Objective

Wire four primitives for one real workflow: a project-reviewer persona (skill), a subagent that uses that persona with tool access, your commit-style skill for the commit message, and a `/review-and-commit` command that orchestrates them.

## Why cumulative

This is the first exercise requiring you to hold multiple primitives simultaneously. The gate failure to watch for: building them in isolation and never wiring them. *Wiring* is the exercise.

## Midway checkpoint (mandatory)

Before writing the command that orchestrates everything, you must:
1. Build the persona (skill) and verify it triggers on its own.
2. Build the subagent separately and spawn it once on real code.
3. Confirm your S2.1 skill still works as installed.

Document the checkpoint in `submission/checkpoint.md`. Only after each piece works independently should you wire them. This prevents debugging compound failures later.

## Requirements

1. Build a project-reviewer persona as a skill at `submission/project-reviewer/SKILL.md`. It should embody the review lens for *your* codebase (pick a repo you know). Use skill-creator if helpful.
2. Build a subagent definition at `submission/agents/project-reviewer.md`. Same persona, but with tools: `Read, Grep, Glob, Bash`. Clear "done" criterion.
3. Copy your S2.1 commit-style skill into `submission/commit-style/`.
4. Run the midway checkpoint. Save `submission/checkpoint.md`.
5. Build `submission/review-and-commit.md` command with `argument-hint: <file-or-diff>`. It:
   - Spawns the project-reviewer subagent on the diff
   - Uses findings to inform the commit message
   - Applies the commit-style skill
6. Run the whole workflow on a real diff (pick one from your project). Capture in `submission/run-log.md`.
7. Write `submission/reflection.md`:
   - Where did you choose skill vs subagent? Defend each.
   - Did the persona's review differ from a generic review? Cite specifics.
   - What would break if you removed the persona? (Test it.)

## Submission

- `submission/project-reviewer/SKILL.md`
- `submission/agents/project-reviewer.md`
- `submission/commit-style/` (from S2)
- `submission/checkpoint.md`
- `submission/review-and-commit.md`
- `submission/run-log.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 26 / 35 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Persona (skill form)** — valid, project-specific lens | `[GATE]` | /5 | Gate: if generic enough to apply to any codebase, fail. Must be specific. |
| **Subagent** — valid frontmatter, scoped tools, clear done-state | `[GATE]` | /5 | Gate: unrestricted tools = fail. Must scope. |
| **Checkpoint evidence** — each piece verified independently | `[GATE]` | /5 | Gate: skipping checkpoint and going straight to wiring = fail. |
| **Command** — wires the pieces, uses `argument-hint` | — | /5 | Works + scoped tool access |
| **Actual run** — full workflow on real diff | `[GATE]` | /5 | Gate: simulated or mocked = fail. Real diff, real run. |
| **Counterfactual** — tested with persona removed | — | /5 | Actually ran it without the persona, documented the delta |
| **Integration judgment** — defended each primitive choice | — | /5 | Why skill here, subagent there — with trade-off citations |

## What "good" looks like

Your run log shows the subagent catching project-specific concerns ("this component imports from `legacy/` — our convention is to feature-flag any new legacy imports"). The counterfactual test reveals that without the persona, Claude writes generic "looks good" reviews that would have missed two real issues.

## Integration with prior sections (targeted)

- **Section 2 — load-bearing.** Your commit-style skill is reused. Grader verifies it's the S2 version.
- **Section 3 — load-bearing.** Your command-writing muscles from S3 are directly applied.
- Section 1 (mental models) is not explicitly required here — it's background.
