# Section 2 — Skills (with Eval Intro)

**Source:** `agentic-learning-plan-v3.md` → Phase 2 + eval intro (addendum from Phase 7)
**Prerequisites:** Section 1 complete
**Estimated time:** 2–3 hours

## What you'll walk away knowing

- `SKILL.md` anatomy — frontmatter, body, optional `scripts/` and `references/`
- Why the **description field is everything** for trigger reliability
- **Progressive disclosure** — reference files stay out of context until needed
- How to use `skill-creator`'s built-in eval to measure trigger reliability and iterate

## Concepts the teacher will work through with you

1. **Skill anatomy.** Folder + `SKILL.md` + optional `scripts/` + optional `references/`.
2. **Frontmatter: `name` and `description`.** Description scanned at session start (~100 tokens per skill). Includes the trigger phrases users actually type.
3. **Weak vs strong descriptions.** Compare the two examples in Phase 2 of the source. Mechanism: action verbs + artifact nouns + "use even when" escape clauses.
4. **Progressive disclosure.** Reference files load only when needed.
5. **Eval as build habit.** `skill-creator` includes a trigger-reliability eval. Run it while building, not after shipping.

## Check-for-understanding questions

- If you wrote a skill with `description: "How to write API endpoints"`, what prompt types would fail to trigger it?
- Why put knowledge in `references/` instead of the SKILL.md body?
- You have a skill that triggers on feature work but misses on bugs. One change to fix that?
- What does skill-creator's eval actually measure, and what can't it measure?

## Exercises

- **01-basic:** Build a commit-message skill using skill-creator. Run its built-in eval. Iterate.
- **02-cumulative:** Build a second skill for a task from *your* Section 1 anti-patterns list that you realized *was* worth encoding. Integrates Section 1's mental model.
