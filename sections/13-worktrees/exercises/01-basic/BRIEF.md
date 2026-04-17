# Exercise 13.1 — Two Parallel Worktrees

**Time estimate:** 1–1.5 hours
**Dependencies:** Claude Code v2.1.49+ (`claude --version` to check); git

## Objective

Run two Claude sessions in parallel worktrees on independent tasks. Feel the difference between sequential and truly parallel.

## Setup

```bash
# Verify version supports worktrees
claude --version   # v2.1.49 or later
```

If version is older, upgrade first. Older versions can still use `git worktree add` manually but lose the `--worktree` flag convenience.

## Requirements

1. Pick two truly independent tasks from real backlog. They **must touch different files** — otherwise you're testing merge handling, not worktrees.
2. Open two terminals:
   ```bash
   # Terminal 1
   claude --worktree task-one

   # Terminal 2
   claude --worktree task-two
   ```
3. Start both simultaneously. Monitor both.
4. Capture:
   - Worktree paths (usually `.claude/worktrees/task-<name>/`)
   - What each session accomplished: `submission/task-one-log.md`, `submission/task-two-log.md`
   - Wall-clock time start → done for each
5. Write `submission/reflection.md`:
   - How did your attention split? Worth it?
   - What broke? (Shared resources — ports, DB, env, running services?)
   - When would you reach for worktrees next?

## Submission

- `submission/task-one-log.md`
- `submission/task-two-log.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Two real independent tasks** — different files | `[GATE]` | /5 | Gate: tasks touching same file = fail |
| **Genuinely parallel** — wall-clock evidence | `[GATE]` | /5 | Gate: sequential execution dressed up as parallel = fail |
| **Honest problem report** — named what broke | — | /5 | |
| **Reach-for heuristic** — when to use, when not to | — | /5 | |

## What "good" looks like

Task A finishes in 22 min on branch `worktree-task-one`. Task B finishes in 35 min on `worktree-task-two`. Both ran simultaneously. You noted that your local Postgres had migrations applied by both — conflict. Solution noted: per-worktree schema prefix. Reflection: *"Worktrees next time I have two independent features and can afford the attention split. Not for work I need to tightly monitor."*

## Integration with prior sections

None — introduces worktree mechanics.
