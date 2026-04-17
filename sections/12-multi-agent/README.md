# Section 12 — Multi-Agent Thinking

**Source:** `agentic-learning-plan-v3.md` → Phase 5
**Prerequisites:** Sections 1–11 complete
**Estimated time:** 2.5 hours

## What you'll walk away knowing

- Mental shift: single agent = pair-programming. Multi-agent = managing a team.
- Subagents: handoff from main to an isolated helper with tools and "done" criteria
- Agent Teams: true parallel execution via `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=true`
- Why parallel agents often *outperform* a solo session (each stays fresh)
- The cost of coordination — when multi-agent loses to a focused single agent

## Concepts the teacher will work through with you

1. **Multi-agent is an economic choice.** Parallelism and specialization vs overhead and coordination cost. Small tasks lose.
2. **Subagents are the gateway drug.** You stay in control. The helper runs in isolation and returns a summary.
3. **Agent Teams for true parallelism.** Three specialists working simultaneously on different facets. Lead synthesizes.
4. **Clean contexts beat long contexts.** Each agent starts fresh. No accumulated drift.
5. **Synthesis is the value.** Concatenating reviewer outputs is worthless. Resolving conflicts and producing a recommendation is the point.

## Check-for-understanding questions

- When would you use a subagent vs Agent Teams?
- What's the specific failure mode of spawning too many parallel agents?
- Give a case where a solo session outperforms a three-agent review.

## Exercises

- **01-basic:** Spawn one subagent for a scoped, tool-heavy task.
- **02-cumulative:** Three-reviewer PR team with synthesis. *Integrates S4, S11.*
