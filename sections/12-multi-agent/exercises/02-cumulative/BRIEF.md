# Exercise 12.2 — Cumulative: Three-Reviewer PR Team

**Time estimate:** 2–2.5 hours
**Dependencies:** Your S4.2 project-reviewer persona; S11 forking mechanics
**Integrates:** S4 (personas as subagents), S11 (forks vs agents distinction)

## Objective

Review a real PR with three specialized subagents — security, performance, test-coverage — running in parallel. Synthesize conflicts and tensions into a single recommendation.

## Why cumulative

The value here is *composition*, not any single primitive. S4 taught you to build scoped personas. S11 taught you when to fork. Multi-agent review combines both: three scoped personas running in isolation.

## Requirements

1. Copy starter:
   ```bash
   cp -r _starter-project/ submission/project/
   ```
2. Add three subagent definitions at `submission/project/.claude/agents/`:
   - `security-reviewer.md` — build on your S2.2 security skill if you have one; or new
   - `performance-reviewer.md` — N+1 queries, re-renders, bundle size, hot paths
   - `test-reviewer.md` — coverage gaps, edge cases, flaky test patterns
3. Each subagent has scoped tools (`Read, Grep, Glob` at minimum; `Bash` only if needed).
4. Pick a real PR. Requirements:
   - Open-source, your own work, or synthesized from a real diff
   - Substantive (>100 lines changed across >3 files)
   - At least one non-trivial issue in *each* reviewer's lens (otherwise some reviewers return "LGTM" and the exercise is dull)
   - Save as `submission/pr-diff.md`
5. Run all three reviewers. Each writes to `submission/reviews/<name>.md`.
6. **Synthesis step (this is where the value is):** in your main session, produce `submission/synthesis.md` containing:
   - Agreed issues across ≥2 reviewers
   - Conflicts between reviewers (security says do X; perf says don't)
   - Trade-off resolution with reasoning
   - Final recommendation (merge / tweak / reject + specifics)
7. Write `submission/reflection.md`:
   - Did any two reviewers surface the same issue with different framings?
   - Where did they genuinely conflict? How did you resolve?
   - What did synthesis add that concat wouldn't?
   - Was the parallel run actually faster than sequential? (Wall-clock evidence.)

## Submission

- `submission/project/` — with three subagents
- `submission/pr-diff.md`
- `submission/reviews/security.md`
- `submission/reviews/performance.md`
- `submission/reviews/test-coverage.md`
- `submission/synthesis.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Three differentiated subagents** — each has a distinct lens | `[GATE]` | /5 | Gate: near-duplicate subagents = fail |
| **Real, substantive PR** — per the requirements | `[GATE]` | /5 | Gate: trivial PR with no cross-lens tension = fail |
| **Parallel execution** — wall-clock evidence | — | /5 | Ran in parallel, captured timings |
| **Synthesis adds value** — not just concat | `[GATE]` | /5 | Gate: a synthesis that's just "here's what each said" = fail. Must resolve or name unresolvable tensions. |
| **Conflict handling** — explicit | — | /5 | Where reviewers disagreed, you chose with reasoning |
| **Reflection** — specific tension named | — | /5 | |

## What "good" looks like

Security flagged a timing attack on token comparison. Performance wanted caching on that same endpoint. Both correct, in tension. Your synthesis: *"Cache the result; preserve `crypto.timingSafeEqual` in the comparison path. Cache lookup is constant-time; the comparison stays constant-time. Both concerns resolved; cached timing attacks remain out of scope since cache key is non-user-controlled."* Reflection: *"This kind of multi-lens tension is exactly what a solo session can't produce — no single persona holds both concerns with equal weight."*

## Integration with prior sections (targeted)

- **S4 — load-bearing.** Persona-as-subagent design + scoped tool allowlists.
- **S11 — load-bearing.** Distinction between fork (research summaries) and subagent (bounded task + actionable output) directly applied.
