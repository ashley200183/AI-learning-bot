# Exercise 13.2 — Cumulative: Batch Migration System

**Time estimate:** 2.5–3 hours
**Dependencies:** Your S2.2 second skill (or any skill encoding a migration pattern); S12 subagent design; S13 worktree mechanics
**Integrates:** S2 (skill encoding the pattern), S12 (subagents as workers), S13 (worktrees for isolation)

## Objective

Build a reusable batch-migration template: a skill encoding the migration pattern, a command that takes a file list, a worktree-isolated subagent that processes each subset, and a merge step.

## Why cumulative

Scale changes what's possible. A solo session can't migrate 20 files without drifting. A batch system can — but only if each piece (skill, subagent, worktree) works reliably. This tests whether S2, S12, and S13 integrate cleanly.

## Requirements

1. Pick a real repetitive change. Examples:
   - Migrate 20 imports from one path to another
   - Add JSDoc type annotations to 15 functions
   - Replace `Date.now()` with a project timestamp helper across 20 files
   - Update error-handling pattern across 20 files
2. Copy starter: `cp -r _starter-project/ submission/project/`
3. Build `submission/project/.claude/skills/migration-pattern/SKILL.md` — encodes the *how* of the migration (rules, edge cases, checks).
4. Build `submission/project/.claude/agents/batch-migrator.md` with `isolation: worktree`. Brief:
   - Accept a list of files
   - Apply the migration skill
   - Commit the change in its worktree
   - Return a summary of what changed and any anomalies
5. Build `submission/project/.claude/commands/batch-migrate.md` with `argument-hint: <target-dir-or-glob>`. Splits files across N agents (N ≤ 5).
6. **Run it** on 10–20 real files. Capture:
   - Per-worktree: branch name, files modified, status → `submission/worktree-log.md`
   - Wall-clock total
   - Failures / conflicts
7. **Merge.** Merge all successful worktree branches. Capture conflicts and resolutions in `submission/merges.md`.
8. Write `submission/reflection.md`:
   - Which worktrees failed and why? (Edge cases the skill missed? Environmental issues? Both?)
   - What would change at 10× scale (100+ files)?
   - Compared to running this as one long solo session, what did you gain and lose?

## Submission

- `submission/project/` — wired setup
- `submission/worktree-log.md`
- `submission/merges.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Real migration ≥10 files** | `[GATE]` | /5 | Gate: <10 files or synthetic = fail |
| **Skill + subagent + command wired** — all three load-bearing | `[GATE]` | /5 | Gate: any one missing or decorative = fail |
| **`isolation: worktree` actually used** — parallel evidence | `[GATE]` | /5 | Gate: sequential run = fail |
| **Merges completed** — honest conflict handling | — | /5 | |
| **Failure analysis** — specific, per-worktree | — | /5 | |
| **Scale reasoning** — what breaks at 10× | — | /5 | Specific bottleneck named, not generic |

## What "good" looks like

4 worktrees each handled 3–5 files. One failed because your migration skill didn't handle default-export files. Your reflection shows the skill-edit that would fix it next time. Merge conflicts: only `package-lock.json` (expected). At 10× scale, you'd split into phases: 10 agents migrating imports first, then 10 updating tests — because running 100 agents touching tests simultaneously corrupts Jest's cache.

## Integration with prior sections (targeted)

- **S2 — load-bearing.** The migration pattern is a skill.
- **S12 — load-bearing.** Subagent design and scoped briefs.
- **S13 — load-bearing.** Worktree isolation.
