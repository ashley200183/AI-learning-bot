# Exercise 14.2 — Cumulative: One Real Tier-2 Run

**Time estimate:** 3–6 hours total (prep + run + debrief)
**Estimated cost:** $20–$80 depending on tool and scope
**Dependencies:** Gas Town, oh-my-claudecode, or equivalent tier-2 tooling; your S5.2 integrated project; your S13.2 batch-migration experience
**Integrates:** S5 (CLAUDE.md as the brief foundation), S13 (worktree isolation tier-2 relies on)

## Cost note

This exercise costs real money. A small well-scoped tier-2 run is typically $20–40. Oversize scope pushes to $60+. If budget is a concern:
- Start with the *smallest* scope that still has ≥3 tasks
- Or use a cost-bounded fork: 2 hours of Claude in a dedicated worktree you supervise more closely, mimicking tier-2 without the supervisor agent. Capture what tier-2 tooling would have automated.

## Objective

Prepare for, execute, and rigorously debrief one tier-2 orchestration run. The preparation is the test of whether Sections 2–13 stuck.

## Part A — Preparation (this is the load-bearing step)

Use the starter:
```bash
cp -r _starter-project/ submission/project/
```

Populate with:
- Your integrated CLAUDE.md from S5.2 — tightened for tier-2 autonomy (what should the agent refuse to do unattended?)
- Your project-scope skills (at least your S2.1 commit-style + one other relevant one)
- Your persona routing rules
- A `notes/current-sprint.md` with sprint context
- A `notes/agent-brief.md` — the specific tier-2 brief

Git status must be clean before launch — commit or stash.

Save the pre-state as `submission/pre-state.md`:
- List of installed skills
- CLAUDE.md filename + size
- Git status (should be clean)
- Content of agent-brief.md

## Part B — Scope

In `submission/scope.md`:
- 3–6 related tasks, well-scoped, lower-risk
- Explicit "done" criterion per task
- Estimated cost / time budget
- What's out of scope

## Part C — Run

Launch the tier-2 tool. Capture:
- Cost progression (check mid-run if the tool supports it)
- Wall-clock time
- PRs opened (or equivalent)
- Outputs per task

→ `submission/run-log.md`

## Part D — Review

Categorize each output: ship-ready / needs tweak / reject. Cite specific issues.

→ `submission/review.md`

## Part E — Debrief (mandatory)

- **CLAUDE.md diff** — at least one rule added per mistake pattern observed → `submission/claudemd-diffs.md`
- **Skill edits** — descriptions tightened where triggering missed → `submission/skill-edits.md`
- **Next-run plan** — what would make the next run 2× better → `submission/next-run-plan.md`

## Part F — Reflection

`submission/reflection.md`:
- Worth it? Cost vs time saved, honestly.
- Which section's prep was most load-bearing?
- What would you skip next time?

## Submission

- `submission/project/`
- `submission/pre-state.md`
- `submission/scope.md`
- `submission/run-log.md`
- `submission/review.md`
- `submission/claudemd-diffs.md`
- `submission/skill-edits.md`
- `submission/next-run-plan.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 26 / 35 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Preparation complete** — all S5 + S13 assets in place | `[GATE]` | /5 | Gate: missing major pieces = run is unfair to agents = fail |
| **Scope well-sized** | — | /5 | |
| **Run evidence real, costed** | `[GATE]` | /5 | Gate: no real run = fail |
| **Review rigor** — each output categorized with specifics | `[GATE]` | /5 | Gate: aggregate review = fail |
| **Debrief: CLAUDE.md diff** — real rule added, tied to observed failure | `[GATE]` | /5 | Gate: no diff = no learning = fail |
| **Debrief: next-run plan** — concrete | — | /5 | |
| **Reflection: worth it?** — honest, specific | — | /5 | |

## What "good" looks like

CLAUDE.md diff: *"Never allow tier-2 agents to touch `/src/payments/` unattended — drift risk on financial logic is too high. Route payments changes through a pre-flight human review."* That rule came from observing Agent 3 confidently rewriting a refund handler in a way that would have refunded twice.

## Integration with prior sections (targeted)

- **S5 — load-bearing.** CLAUDE.md quality is the difference between tier-2 working and tier-2 producing junk.
- **S13 — load-bearing.** Tier-2 tooling runs agents in worktrees; failure modes from 13.2 re-appear here at scale.
