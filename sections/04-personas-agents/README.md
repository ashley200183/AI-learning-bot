# Section 4 — Personas & Agents

**Source:** `agentic-learning-plan-v3.md` → Phase 3
**Prerequisites:** Sections 1–3 complete
**Estimated time:** 2.5–3 hours

## What you'll walk away knowing

- Personas as cognitive framing — they change *what Claude attends to*, not what tools it has
- Agents as role + capability — own context, own tool allowlist, scoped brief
- When to use a persona (advise) vs an agent (act)
- How to build a project-specific persona and subagent

## Concepts the teacher will work through with you

1. **Personas shift attention, not tools.** Same code through `/review` vs `/qa` = different feedback.
2. **Agents extend personas with capability.** Tool allowlist, model choice, definition of "done."
3. **Parent context stays clean.** The subagent's 20K tokens of file reads never touch your main window.
4. **Persona routing.** "Code review → agents/project-reviewer.md" is a one-line rule that changes how all reviews happen.

## Check-for-understanding questions

- You have a code-review persona and a subagent that also reviews code. Give a task for each.
- What can an agent do that a persona can't?
- Why does the parent context stay clean when you spawn a subagent?
- What would happen if you gave a subagent `Bash(*)` with no restrictions?

## Exercises

- **01-basic:** Install gstack, run `/review` and `/qa` on the same code, compare.
- **02-cumulative:** Build a project-reviewer persona + subagent, use with your S2 skill + S3 command. *Integrates S2, S3.*
