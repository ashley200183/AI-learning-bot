# Section 17 — Evals, Benchmarking, Cost & Model Routing

**Source:** `agentic-learning-plan-v3.md` → Phase 7 + merged cost/routing material
**Prerequisites:** Sections 1–16 complete
**Estimated time:** 3–4 hours (includes a real eval suite)

## What you'll walk away knowing

- Evals as automated tests for AI workflows — input/output pairs + grading
- Three grader types: **deterministic** (tests pass), **LLM-as-judge** (rubric), **heuristic** (regex/length/structure)
- The continuous eval loop — re-run on every change, compare scores
- **Cost + model routing as design discipline** — Opus vs Sonnet vs Haiku per task, prompt caching, per-workflow cost budgets
- Peer-grading — develop your eye for what "good" looks like by grading someone else's work

## Concepts the teacher will work through with you

1. **Deterministic beats LLM-judge where possible.** "Tests pass" > "looks good."
2. **LLM-judge rules.** Don't self-grade (self-preference bias), define concrete criteria, calibrate with humans periodically.
3. **Heuristics as pre-filter.** Fast, free, catches the obvious. Full LLM-judge only on what passes heuristics.
4. **Cost is a design axis.** Haiku for quick classification, Sonnet for most work, Opus for decisions that matter. Prompt caching for long skill bodies.
5. **Model routing in practice.** A workflow with 80% classifications (Haiku) and 20% judgment calls (Opus) costs 5× less than routing everything through Opus.
6. **Peer-grading is retrieval practice.** Grading someone else's work sharpens your taste faster than producing your own.

## Check-for-understanding questions

- Commit-message skill: which grader types apply? What does each measure?
- Why shouldn't the same model grade its own output?
- Your eval is $2/run. You have 30 skills. What's your budget strategy?
- When does Haiku beat Sonnet on cost-adjusted quality? When does it lose badly?

## Exercises

- **01-basic:** Run skill-creator's eval on your skill. Iterate to plateau.
- **02-cumulative:** Full eval suite + peer-grading + cost-routing design. Capstone. *Integrates S2, S5, S8 (+ any other skill-producing sections).*
