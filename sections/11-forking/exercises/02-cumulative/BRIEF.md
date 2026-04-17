# Exercise 11.2 — Cumulative: Exploratory Main + Fork Execution

**Time estimate:** 90 minutes (plus one comparable inline session ~60 min)
**Dependencies:** Your S11.1 deep-research fork; S6 context-management habits
**Integrates:** S6 (in-session context management), S11 (forking)

## Objective

Run a 60–90 minute session with a strict rule: **decisions in main, executions forked.** Pair with one comparable inline session to measure the difference cleanly.

## Scenario design (critical — do this first)

Pick a task that is *known to cause context drift if done inline*. Specifically:
- Requires ≥2 research phases (forking territory)
- Requires ≥1 substantive implementation (forking territory)
- Requires ≥1 real-time decision where Claude might drift from stated constraints

Save as `submission/scenario.md`. If your scenario doesn't force drift, the comparison is invalid.

## Requirements

### Main session (managed)

1. Run the scenario for 60–90 min with the rule: main is exploratory only. Every file read, every implementation, every research dive → fork or subagent.
2. Maintain `submission/main-log.md` — every turn of the main session's conversation.
3. Per-fork: capture the brief given + the return + `/cost` delta in `submission/forks/<task>.md`.

### Inline session (baseline)

1. Fresh context. Same scenario. Work inline, no forks.
2. Capture `submission/inline-log.md`.

### Analysis

`submission/analysis.md`:
- Total tokens: main + forks combined vs inline
- **Consistency check:** count concrete instances where inline Claude contradicted an earlier decision. Count the same in the main+fork version. (This is the point — main contexts that stay high-level don't drift.)
- Wall-clock time for each (forks run fast; inline is continuous)

`submission/reflection.md`:
- What's the *time cost* of forking (round-trips) vs inline? Worth it given the coherence gain?
- One specific moment where forking let the main session stay high-level that inline would have lost.
- If you had to re-do this exercise, what would you fork differently?

## Submission

- `submission/scenario.md`
- `submission/main-log.md`
- `submission/forks/` — all fork briefs + returns
- `submission/inline-log.md`
- `submission/analysis.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Scenario is drift-prone** — designed to cause context issues if done inline | `[GATE]` | /5 | Gate: trivial scenario = no contrast = fail |
| **Main stayed exploratory** — no raw file contents, decisions only | `[GATE]` | /5 | Gate: if main log includes the forked work verbatim, fail (defeats the point) |
| **Both sessions real and comparable** | `[GATE]` | /5 | Gate: inline omitted or only half-done = fail |
| **Token comparison** — real numbers | — | /5 | |
| **Consistency count** — counted contradictions in both | — | /5 | Specific instances cited |
| **Reflection** — specific moment named | — | /5 | |

## What "good" looks like

Inline session: Claude drifted twice (proposed raw SQL after you'd specified Prisma; suggested a tight loop after you'd specified async). Main+fork: zero drift — the decision layer only saw summaries, never the implementation noise that would have distracted it. Token cost was roughly equivalent total, but main-context tokens dropped 85%.

## Integration with prior sections (targeted)

- **S6 — load-bearing.** Context management habits are what makes the main session stay clean.
- **S11 — load-bearing.** Fork mechanic is the tool.
