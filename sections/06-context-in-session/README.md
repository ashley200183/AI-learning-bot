# Section 6 — Context Windows & In-Session Management

**Source:** `agentic-learning-plan-v3.md` → Phase 3.5 (part A)
**Prerequisites:** Sections 1–5 complete
**Estimated time:** 1.5 hours

## What you'll walk away knowing

- Why context degradation is real — Claude contradicts earlier decisions when the window fills, even though the instructions are still in it
- Three in-session strategies: **compaction**, **tool-result clearing**, **scope containment** via `/btw`
- How to steer compaction: `/compact Focus on X`
- Why tool-result clearing is the biggest lever in long sessions (80% of tokens)

## Concepts the teacher will work through with you

1. **Context degradation is real.** The 8am-vs-midday example from the source: Claude *has* the instructions but behaves as if it doesn't.
2. **Compaction distills.** Auto at ~80% capacity, or manual with `/compact Focus on X`.
3. **Tool results are the hidden bulk.** File reads, bash outputs, web fetches pile up. Most are re-fetchable — drop them.
4. **`/btw` as a scope-containment tool.** Questions in an overlay that never enter history.

## Check-for-understanding questions

- Why does a 5-hour session degrade even though the instructions are still *in* the window?
- You're at 150K tokens, 20 tool calls deep, and things feel slow. What's your first move?
- When would `/btw` fail you (i.e., you *want* it in history)?

## Exercises

- **01-basic:** Run the same prompt sequence twice — unmanaged then managed. Measure delta.
- **02-cumulative:** Design a `/compact-smart` command that uses your S5 CLAUDE.md priorities. *Integrates S3, S5.*
