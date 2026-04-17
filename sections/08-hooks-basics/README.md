# Section 8 — Hooks Basics

**Source:** `agentic-learning-plan-v3.md` → Phase 4.5 (part A)
**Prerequisites:** Sections 1–7 complete
**Estimated time:** 2 hours

## What you'll walk away knowing

- Hooks are *deterministic* — they always fire. That's the defining property.
- Lifecycle events: `SessionStart`, `UserPromptSubmit`, `PreToolUse`, `PostToolUse`, `Stop`, `Notification`
- Exit codes as control flow: 0 = proceed, 2 = block (stderr → Claude), other non-zero = warn
- The difference between CLAUDE.md (hope), skills (matching), and hooks (certainty)

## Concepts the teacher will work through with you

1. **Deterministic means it always runs.** No semantic matching. No "Claude decided not to."
2. **`SessionStart` and `UserPromptSubmit` inject context** — their stdout reaches Claude. Other events log only.
3. **Exit 2 is a power move.** It blocks the tool call AND Claude sees your stderr. Turns a script into a policy engine.
4. **The starter three:** sprint-context injection, auto-format on edit, simple PreToolUse guard (one kind of command).

## Check-for-understanding questions

- Why can't a CLAUDE.md rule replicate what a `PreToolUse` hook does?
- Exit 1 vs exit 2 — what does Claude see and do in each?
- Give an example where a hook would be *overkill* and a CLAUDE.md rule is enough.

## Exercises

- **01-basic:** Install one hook (lab-style), watch it fire, understand the event payload. Then build two more.
- **02-cumulative:** Hook that injects your S5.2 forced-eval. *Integrates S5.*
