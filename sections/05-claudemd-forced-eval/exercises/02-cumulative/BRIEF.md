# Exercise 5.2 — Cumulative: Wire Skill + Command + Persona

**Time estimate:** 2–2.5 hours
**Dependencies:** Your S2.1 skill, S3.1 command, S4.2 persona/subagent
**Integrates:** S2 (skill), S3 (command), S4 (persona/subagent)

## Objective

Put a real integrated project together: S2 commit-style + S3 custom command + S4 project-reviewer (persona + subagent), all coordinated by a CLAUDE.md. Design one end-to-end scenario that triggers at least three of them in one session.

## Why cumulative

Until now you've built primitives that happened to coexist. This is the integration test. If the primitives only fire in their own demos but don't cooperate when wired, your CLAUDE.md is the weak link.

## Setup — use the starter project

Copy the starter template:
```bash
cp -r _starter-project/ submission/project/
```

Then populate it with your prior-section artifacts:
- `submission/project/.claude/skills/commit-style/` ← from S2.1
- `submission/project/.claude/skills/project-reviewer/` ← from S4.2
- `submission/project/.claude/commands/<your-s3-command>.md` ← from S3.1
- `submission/project/.claude/agents/project-reviewer.md` ← from S4.2

## Requirements

1. Copy prior-section artifacts into the starter structure above.
2. Write `submission/project/CLAUDE.md` with:
   - Project context
   - Forced-eval block naming *your specific* skills (not generic)
   - Priority rule for when multiple skills might trigger (e.g., review before commit)
   - Persona routing
   - Compaction preservation
   - Anti-patterns
3. Design **one end-to-end scenario** in `submission/scenario.md` — one realistic task that should trigger ≥3 of your primitives in one session. Example: *"Review this diff, write an appropriate commit message, and produce release notes for the change."*
4. Run the scenario. Capture full transcript in `submission/run.md`. Note which primitives fired, which didn't.
5. Iterate the CLAUDE.md once based on observed failures. Re-run. Capture second run in `submission/run-v2.md`.
6. Write `submission/reflection.md`:
   - What triggered automatically? What needed explicit invocation?
   - Did any two primitives conflict (same trigger territory)?
   - **Counterfactual:** delete CLAUDE.md and rerun. Which primitives still fire? Document in `submission/without-claudemd.md`.

## Submission

- `submission/project/` — full wired setup
- `submission/scenario.md`
- `submission/run.md`
- `submission/run-v2.md`
- `submission/without-claudemd.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Prior artifacts correctly installed** — actually in place, loadable | `[GATE]` | /5 | Gate: if skills/commands don't load, nothing else matters. |
| **CLAUDE.md coherence** — priorities clear, no contradictions | — | /5 | Handles "two skills could trigger" case explicitly |
| **Forced-eval specificity** — names YOUR skills, not generic | `[GATE]` | /5 | Gate: "use applicable skills" fails. Must name your specific artifacts. |
| **End-to-end run** — ≥3 primitives fire in one scenario | `[GATE]` | /5 | Gate: evidence of ≥3 primitives actually firing. |
| **Iteration** — real change, re-run shows delta | — | /5 | Targeted at observed failure |
| **Counterfactual run** — tested with CLAUDE.md removed | — | /5 | Real test, compared outcomes |

## What "good" looks like

Your `run.md` shows the forced-eval block catching that both `commit-style` and your new `handoff-writer` could fire on "let's wrap this up with a commit." Your CLAUDE.md now has explicit priority. Your counterfactual shows that without CLAUDE.md, only 1 of 3 primitives triggered — the rest stayed dormant.

## Integration with prior sections (targeted)

- **S2 — load-bearing.** Your commit-style skill.
- **S3 — load-bearing.** Your custom command.
- **S4 — load-bearing.** Your persona + subagent.
- S1 is background; not explicitly required here.
