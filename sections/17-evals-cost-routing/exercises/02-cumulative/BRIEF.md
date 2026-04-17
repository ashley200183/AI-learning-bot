# Exercise 17.2 — Cumulative: Full Eval Suite + Peer-Grading + Cost Routing

**Time estimate:** 4–5 hours (this is the capstone)
**Dependencies:** One skill you've iterated on; access to multiple Claude models (Haiku / Sonnet / Opus); optional: Python + `anthropic` SDK, or Claude Code native eval primitives
**Integrates:** S2 (the skill being evaluated), S5 (CLAUDE.md can enforce "run evals on skill changes")

## Objective

Three things in one capstone:
1. **Build a full eval suite** for one of your skills with all three grader types + cost tracking
2. **Peer-grade** a sample submission (provided below) to sharpen your eye for quality
3. **Design a cost + model routing strategy** for one of your multi-step workflows from earlier sections

## Part 1 — Eval Suite

### Tool choice

You have two paths:
- **Python + Anthropic SDK** — full control, more code. Use this if you want the portability.
- **Claude Code native** — spawn Claude sessions via `claude -p` and collect outputs. Less code, less portable.

Pick one and say why in `submission/suite/CHOICE.md`.

### Requirements

Build at `submission/suite/`:

```
suite/
  cases/
    case-001.json      ← input + expected_properties
    ...
    case-010.json      ← minimum 10
  graders/
    heuristics.{py|sh}
    deterministic.{py|sh}
    llm_judge.{py|sh}
  run_eval.{py|sh}
  compare.{py|sh}
  results/
    run-v1-<hash>.json
```

- **Heuristic grader:** regex/length/required-terms/required-format checks
- **Deterministic grader:** runs a test or validator that passes/fails (e.g., does the output parse as valid Conventional Commits?)
- **LLM-judge grader:** rubric-based. Use a *different model* than the one being graded where possible (e.g., Haiku grading Sonnet output). Rubric outputs JSON scores.
- **Cost tracking:** per-case input/output tokens, model used, cost in USD, latency

### Runs

- Run the suite on v1 of the skill.
- Make one targeted change.
- Run on v2.
- Compare in `submission/suite/analysis.md`: per-grader per-case deltas, regressions detected, progressions.

## Part 2 — Peer-Grading

In `submission/peer-grading/`:

A fictional student's submission for Exercise 2.1 (building a commit-message skill):

```yaml
# commit-helper/SKILL.md
---
name: commit-helper
description: Helps with git commits
---

When the user commits, write a good commit message.
Use conventional format.
```

They claim 4/5 trigger rate on their 5 prompts.

**Your task:** grade this using the S2.1 rubric. Write `submission/peer-grading/GRADE.md` in the format the grader subagent uses. Enforce gates. Be honest.

Then write `submission/peer-grading/reflection.md`:
- Did you pass or fail this submission? Why?
- What would you tell the student to fix first?
- How does grading someone else's work differ from evaluating your own?

## Part 3 — Cost Routing Design

Pick one of your multi-step workflows — S4.2's review-and-commit flow, or S12.2's three-reviewer team, or S13.2's batch-migration system.

In `submission/routing/`:

1. `breakdown.md` — decompose the workflow into its steps. For each step, note:
   - What's being asked (classification? generation? judgment?)
   - Input/output size estimate
   - Quality criticality (is a wrong answer recoverable?)
2. `routing-plan.md` — assign a model to each step:
   - Haiku for quick classifications, simple parsing, length-heavy low-stakes work
   - Sonnet for most generation and routine reasoning
   - Opus for decisions, complex synthesis, ambiguous cases
3. `cost-estimate.md` — back-of-envelope:
   - All-Opus cost of one workflow run
   - Routed cost
   - Percentage savings
4. `routing-skill.md` — a skill or command that, if invoked, uses the routing plan. (Doesn't need full implementation — design-level.)

## Submission

- `submission/suite/` — full eval suite + analysis
- `submission/peer-grading/GRADE.md` + `reflection.md`
- `submission/routing/breakdown.md` + `routing-plan.md` + `cost-estimate.md` + `routing-skill.md`
- `submission/reflection.md` — capstone reflection (below)

## Rubric

Pass: **total ≥ 38 / 50 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **10+ eval cases including edge cases** | — | /5 | Diverse, realistic |
| **Three grader types implemented and working** | `[GATE]` | /5 | Gate: any grader missing or non-functional = fail |
| **LLM-judge uses different model than graded** | `[GATE]` | /5 | Gate: self-grading = fail (the point is bias avoidance) |
| **Cost tracking real** — per-case, per-grader | — | /5 | |
| **v1 vs v2 comparison** — real change, real delta | `[GATE]` | /5 | Gate: no v2 or cosmetic change = fail |
| **Peer-grading: correct call** — this submission should fail | `[GATE]` | /5 | Gate: passing this submission = fail (the sample has a weak description and empty body; rubric says fail) |
| **Peer-grading: specific feedback** — actionable | — | /5 | |
| **Routing: decomposition quality** | — | /5 | Real steps, honest difficulty labeling |
| **Routing: cost estimate honest** | — | /5 | Real numbers, uses current pricing |
| **Capstone reflection** — what changed in your approach | — | /5 | |

## Capstone reflection prompts

In `submission/reflection.md`:
- Where did LLM-judge disagree with deterministic? When they disagree, who wins?
- What's your plan to keep this eval alive — not let it bitrot?
- What skill would you build an eval for *next*, and why that one?
- How did peer-grading change your self-grading?
- What's the most surprising cost savings in your routing plan?

## What "good" looks like

Your v2 passes 9/10 cases; v1 passed 8/10. The regressed case is a prompt where tightened description lost an edge-case trigger — you decide to live with it because v2's total quality is higher. LLM-judge and heuristic disagree on case 7 — LLM says pass (intent matches); heuristic says fail (Conventional Commits format broken). You rule heuristic wins: format compliance is the point of the skill. Peer-grading: you fail the sample submission. The description has no trigger phrases; the body has no structure; no evidence of iteration. Score: 4/20. Clear failure on two gate criteria. Routing: the three-reviewer team has synthesis as its only Opus step; classification of "is this issue security-related" can be Haiku; first-pass review can be Sonnet. Estimated savings: 65% per run.

## Integration with prior sections (targeted)

- **S2 — load-bearing.** The skill being evaluated.
- **S5 — optional.** Can add a CLAUDE.md rule to run evals on skill-file changes.
- **Whichever workflow you chose for routing.**
