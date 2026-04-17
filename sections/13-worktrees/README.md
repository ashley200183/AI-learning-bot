# Section 13 — Git Worktrees

**Source:** `agentic-learning-plan-v3.md` → Phase 5.75
**Prerequisites:** Sections 1–12 complete
**Estimated time:** 2 hours

## What you'll walk away knowing

- Git worktrees give each Claude session its own filesystem + branch + context — true parallel isolation
- `claude --worktree <name>` is native; `isolation: worktree` in agent frontmatter makes it default
- Three patterns: batch migration, competitive implementation, feature + hotfix in parallel
- Gotchas: shared DB/env/ports/services are NOT isolated; every worktree needs its own setup

## Concepts the teacher will work through with you

1. **Worktrees isolate filesystem, not resources.** DB conflicts, port clashes, env vars — those are on you.
2. **Practical limit: 3–10 parallel.** Past that, management overhead dominates.
3. **`isolation: worktree` as default for file-modifying agents.** Parallel safety by construction.
4. **Batch migration scales via worktrees.** N agents, N worktrees, N file-subsets. No file conflicts.

## Check-for-understanding questions

- Why does a parallel worktree often stay more coherent than a single 3-hour session?
- Five agents migrating 50 files across worktrees — what can still conflict?
- When would you *not* use a worktree?

## Exercises

- **01-basic:** Run two parallel worktrees on independent tasks.
- **02-cumulative:** Batch migration with worktree-isolated subagents. *Integrates S2, S12, S13.*
