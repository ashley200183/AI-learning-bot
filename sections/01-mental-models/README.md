# Section 1 — Mental Models + When NOT to Use Agents

**Source:** `agentic-learning-plan-v3.md` → Phase 1 (plus new material on when not to use agents)
**Prerequisites:** None
**Cumulative exercise:** N/A (first section)
**Estimated time:** 1–2 hours

## What you'll walk away knowing

- **Skill** vs **persona** vs **agent** — what each is, how they differ, how they compose
- When to use which primitive (and when to use none)
- **When NOT to use agents** — the reverse heuristic that keeps you from over-applying your new toolkit

## Concepts the teacher will work through with you

1. **Skills as cheat sheets with trigger conditions.** Claude scans descriptions at session start, loads the body only when a task matches. Progressive disclosure — tokens only when needed.
2. **Personas as cognitive framing.** Change *what Claude attends to*, not what tools it has. Same input, different output.
3. **Agents as role + capability.** Own context, own tools, own definition of "done." Parent sees the summary, not the trace.
4. **How they compose.** Skills = procedure. Personas = judgment. Agents = go do. In practice they stack.
5. **When NOT to reach for any of them:**
   - One-off tasks you'll never repeat → just ask Claude directly
   - Tasks that are cheaper to do manually than to scope for an agent
   - Tasks where you can't articulate success criteria
   - Tasks requiring real-time guidance (agents work best in isolation)
   - High-stakes decisions that need you in the loop

## Check-for-understanding questions

- In your own words: what's the difference between a skill's description and its body?
- Same code, two outputs — what's the mechanism?
- Why would you use a subagent instead of just running the task yourself?
- Give me a task where reaching for an agent would be *wrong*. Why?

## Exercise

- **01-basic:** Install the Anthropic skills plugin, observe skill auto-triggering, diagnose failures, and propose three real tasks for which you'd *avoid* building an agent. See `exercises/01-basic/BRIEF.md`.
