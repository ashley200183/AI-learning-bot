# Section 7 — Cross-Session Memory & Handoffs

**Source:** `agentic-learning-plan-v3.md` → Phase 3.5 (part B)
**Prerequisites:** Sections 1–6 complete
**Estimated time:** 2 hours

## What you'll walk away knowing

- Three memory locations: `CLAUDE.md` (standing), `notes/` files (explicit), memory tool (structured, API-side)
- The handoff doc pattern — session-end protocol, session-start protocol
- Agent-to-agent handoff tactics: structured docs, shared state files, git history as memory, CLAUDE.md inheritance
- When handoffs outperform continuing a single long session

## Concepts the teacher will work through with you

1. **Session death is not the same as context fill.** Session ends → notepad discarded. Memory files outlive the session.
2. **`notes/session-handoff.md` is the simplest durable pattern.** CLAUDE.md instructs Claude to update it at session end and read it at session start.
3. **Git history as memory** — often more accurate than prose handoffs for "what changed."
4. **Multi-agent handoffs** — structured doc + shared state file + git log.

## Check-for-understanding questions

- Why is a 9-section summary better than "pick up where we left off"?
- What belongs in `CLAUDE.md` vs `notes/session-handoff.md`?
- If a fresh session reads only your handoff doc, what's the *one* thing that gets lost? (This is what you add next.)

## Exercises

- **01-basic:** Write a handoff protocol (skill + command) and test it with a `/clear`.
- **02-cumulative:** Build a handoff-reviewer persona. *Integrates S2, S3, S4, S5.*
