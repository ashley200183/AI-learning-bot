# Section 5 — CLAUDE.md & Forced-Eval

**Source:** `agentic-learning-plan-v3.md` → Phase 4
**Prerequisites:** Sections 1–4 complete
**Estimated time:** 2 hours

## What you'll walk away knowing

- `CLAUDE.md` as always-loaded standing orders — the right home for project context, conventions, persona routing
- The **forced-eval pattern** — requiring Claude to externalize its skill evaluation. Lifts trigger reliability from ~40% → ~84%.
- What belongs in CLAUDE.md vs a skill vs (eventually) a hook

## Why this section is earlier than the original plan

Memory and handoff patterns (Sections 6–7) live *inside* CLAUDE.md. You need the mechanism before you can discuss using it for persistence. So CLAUDE.md comes first.

## Concepts the teacher will work through with you

1. **CLAUDE.md is always loaded.** Unlike skills, no trigger required.
2. **Forced-eval externalizes the decision.** "List applicable skills, state choice, then proceed." Once in the response, it can't be silently skipped.
3. **Three-way decision (preview):** CLAUDE.md (passive), skills (semi-active matching), hooks (deterministic — Section 8). Use each for what it's good at.

## Check-for-understanding questions

- Why does forced-eval work better than "remember to use your skills"?
- What goes in CLAUDE.md vs a skill body?
- Describe the compaction preservation rules you'd put in CLAUDE.md for *this* learning-bot project.

## Exercises

- **01-basic:** Write a CLAUDE.md with forced-eval for your own project. Measure trigger rate before/after.
- **02-cumulative:** Wire your S2 skill + S3 command + S4 persona into one CLAUDE.md. Prove it with one end-to-end scenario. *Integrates S2, S3, S4.*
