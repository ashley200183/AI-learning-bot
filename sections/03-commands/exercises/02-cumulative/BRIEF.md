# Exercise 3.2 — Cumulative: Skill + Command Pair

**Time estimate:** 1–1.5 hours
**Dependencies:** Your Section 2.1 commit-style skill
**Integrates:** Section 2 (the skill), Section 3 (the command)

## Objective

Take your Section 2 commit-style skill and create a companion `/commit-draft` command with `disable-model-invocation: true`. Show with concrete scenarios when each form wins.

## Why cumulative

Forces you to hold both mental models at once: implicit skill triggering (S2) and explicit command invocation (S3). If you can't articulate the difference via concrete examples, you don't actually understand the unified system.

## Requirements

1. Copy your Section 2.1 skill into `submission/commit-style/` (copy — don't symlink).
2. Build `submission/commit-draft.md` — same underlying behavior, `disable-model-invocation: true` so it never auto-triggers.
3. Design 4 scenarios in `submission/scenarios.md`:
   - 2 where the **skill** wins (why?)
   - 2 where the **command** wins (why?)
4. Run all 4 scenarios against the right form. Capture results.
5. **Run the same-description experiment:** give both the same description temporarily, invoke a typical commit flow, observe what happens. Document in `submission/experiment.md`. Revert afterward.
6. Write `submission/reflection.md`:
   - When is explicit invocation *worse* than auto-triggering? Vice versa?
   - What did the same-description experiment show?
   - Could you ship just the skill? Just the command? What's lost either way?

## Submission

- `submission/commit-style/` (copy from S2)
- `submission/commit-draft.md`
- `submission/scenarios.md`
- `submission/experiment.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 19 / 25 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Both artifacts present and differentiated** | `[GATE]` | /5 | Gate: copy-paste with a flag flip = fail |
| **Scenario design** — realistic, exercises the difference | — | /5 | 4 specific scenarios teaching distinct trade-offs |
| **Scenario runs** — captured, honest | `[GATE]` | /5 | Gate: no hand-waved "it worked" — real output |
| **Same-description experiment** — actually run and reported | — | /5 | Tested, observed, explained |
| **Integration judgment** — clear when-to-use heuristic | — | /5 | Rule of thumb + honest edge case |

## What "good" looks like

Your reflection lands on something like: *"Use the skill when format enforcement should survive my forgetting to invoke. Use the command when I need to handcraft a release commit and don't want Claude's auto-suggestion polluting my intent. Same description makes the skill fire first — the command becomes dead weight."*

## Integration with prior sections

**Section 2 — targeted.** Your S2 commit-style skill is directly reused. Grader verifies it's actually your S2 work, not a quickly-written version.
