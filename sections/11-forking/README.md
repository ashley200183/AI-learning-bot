# Section 11 — Forking Conversations

**Source:** `agentic-learning-plan-v3.md` → Phase 5.5
**Prerequisites:** Sections 1–10 complete
**Estimated time:** 2 hours

## What you'll walk away knowing

- `context: fork` in skill/command frontmatter spawns a separate agent for execution; only the summary comes back
- The **exploratory main / execution branch** pattern — main stays high-level, forks do the heavy lifting
- Forking vs subagent vs `/btw` vs inline — the decision matrix

## Why forking comes before multi-agent

Forking is one agent with different context — simpler. Multi-agent is many agents coordinating — compound. Learning isolation before coordination makes the leap to multi-agent natural.

## Concepts the teacher will work through with you

1. **Fork summarizes, doesn't dump.** Main context gets the result — not the file contents, tool calls, or reasoning trace.
2. **Main for decisions, forks for execution.** Hour-long sessions stay coherent because implementation noise never pollutes the decision layer.
3. **Decision matrix:**
   - Research → fork (Explore agent)
   - Bounded task with output → subagent (S12 will cover)
   - Quick question → `/btw`
   - Complex with real-time guidance → inline
4. **Context savings are real.** A 20K-token research fork returns 500 tokens of summary. Main context barely moves.

## Check-for-understanding questions

- `context: fork` vs spawning a subagent — what's the specific difference?
- You're planning a refactor and want to understand how the current auth flow works. Fork, subagent, or inline?
- Why does the main session's token cost stay low when you fork heavily?

## Exercises

- **01-basic:** Build and use a `deep-research` fork skill.
- **02-cumulative:** Exploratory main + fork execution for a 60-min session. *Integrates S6 (in-session management), S11 (this).*
