# Section 16 — Debugging AI Failures

**Source:** NEW — not covered in the original plan
**Prerequisites:** Sections 1–15 complete
**Estimated time:** 2 hours

## What you'll walk away knowing

- A systematic approach to AI failure modes: is it a **trigger problem**, **context problem**, **instruction problem**, or **capability problem**?
- Diagnostic moves: `/diff`, inspect `.claude/logs/`, read tool-call traces, replay with `/compact` + fewer tools
- When to accept the failure as a limit vs iterate on the prompt, skill, or workflow
- Common symptoms → likely root cause mapping

## Why this section exists

Sections 1–15 assume things work. In practice they don't, and students who can't diagnose their own failures will blame "Claude is bad" and give up. Debugging is the difference between "this didn't work" and "oh, my description didn't include the verb 'migrate' so the skill didn't trigger."

## Concepts the teacher will work through with you

1. **Four failure classes:**
   - **Trigger:** skill/command didn't fire when it should have. Diagnosis: review description, add trigger phrases, test.
   - **Context:** Claude forgot or contradicted earlier state. Diagnosis: check context size, compaction happened, key info out of window.
   - **Instruction:** Claude followed instructions literally but the instructions were wrong. Diagnosis: read what CLAUDE.md / skill actually said, not what you thought it said.
   - **Capability:** Claude genuinely cannot do this (e.g., needs a tool it doesn't have, needs a file it can't access). Diagnosis: check tool allowlist, verify access.
2. **The `/diff` move.** Shows everything Claude changed this session. Often the answer is right there — "I asked it to refactor X but `/diff` shows it only touched Y."
3. **Replay discipline.** Reproduce the failure with minimal context. If it still fails with a clean context and explicit instructions, it's not a context problem — it's an instruction or capability problem.
4. **Log inspection.** Claude Code writes logs to `.claude/logs/`. Skill-eval misses, hook failures, compaction events all live there.

## Check-for-understanding questions

- You asked Claude to use Zod for validation and it used Yup instead. Trigger, context, instruction, or capability problem? How would you tell?
- Skill with `description: "Use for API work"` doesn't fire on "add a new endpoint." What class of failure, and how do you test your hypothesis?
- `/diff` shows Claude modified `file A` and `file B`. You expected only `file A`. What's the first thing you check?

## Exercises

- **01-basic:** Diagnose three planted failures.
- **02-cumulative:** Real-world debugging post-mortem on one of your earlier cumulative exercises. *Integrates any prior sections where you observed failures.*
