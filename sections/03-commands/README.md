# Section 3 — Commands

**Source:** `agentic-learning-plan-v3.md` → Phase 2.5
**Prerequisites:** Sections 1–2 complete
**Estimated time:** 2 hours

## What you'll walk away knowing

- Skills and commands are the *same system* now — differentiated by intent (auto-triggered vs user-invoked)
- The built-ins worth mastering: `/btw`, `/compact Focus on X`, `/diff`, `/loop`, `/cost`, `/model`, `/clear`, `/review`, `/simplify`, `/batch`
- Custom command frontmatter — `allowed-tools`, `argument-hint`, `disable-model-invocation`, `context: fork`, `agent:`
- When to build a command vs a skill for the same capability

## Concepts the teacher will work through with you

1. **Unified system.** `.claude/commands/deploy.md` and `.claude/skills/deploy/SKILL.md` both produce `/deploy`. Difference lives in intent: `disable-model-invocation: true` makes it command-only.
2. **Intent, not mechanism.** Commands = explicit user control. Skills = implicit agent triggering.
3. **Built-ins worth mastering.** `/btw` (overlay questions that don't enter history) is the most underused. `/compact Focus on X` is the most surgical context tool.
4. **Frontmatter as a contract.** `allowed-tools` scopes power; `argument-hint` shapes the UX.

## Check-for-understanding questions

- You want a `/release-notes` capability. Should it be a skill, command, or both? Defend.
- What does `disable-model-invocation: true` actually change?
- When would `/btw` be strictly better than just asking?

## Exercises

- **01-basic:** Build one custom command and use it twice in real work.
- **02-cumulative:** Pair your Section 2 commit-style skill with a companion command. Show when each form wins. *Integrates Section 2.*
