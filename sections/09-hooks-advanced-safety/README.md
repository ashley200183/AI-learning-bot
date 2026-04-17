# Section 9 — Hooks Advanced + Safety

**Source:** `agentic-learning-plan-v3.md` → Phase 4.5 (part B) + new safety material
**Prerequisites:** Sections 1–8 complete
**Estimated time:** 2–2.5 hours

## What you'll walk away knowing

- Advanced hook patterns: `PreCompact` snapshots, `Stop` validators (run tests when Claude claims done), `SubagentStop` output validation
- **Safety-oriented hooks** — production-grade guardrails for touching prod systems, protected files, elevated commands
- Audit trails — hooks as the logging layer for agentic behavior
- When hooks become anti-patterns (infinite loops, performance tar-pits, over-guarding)

## Concepts the teacher will work through with you

1. **`PreCompact` as the save-point pattern.** Snapshot conversation state before compaction destroys it.
2. **`Stop` hook runs tests.** If Claude says "done" and tests fail, the hook blocks with exit 2. Claude sees the failures and continues.
3. **`SubagentStop` validates output.** Gate what subagents return before main session sees it.
4. **Safety layer.** Directory freeze (block edits outside allowed paths), prod-write block, credential-exposure detection.
5. **Anti-patterns.** Hook that spawns Claude that triggers the hook. Hook that runs a 30-second lint. Over-guarding that makes normal work painful.

## Check-for-understanding questions

- Why is `Stop` hook the most useful advanced hook for active development?
- What's the difference between a `PreToolUse` block and a `PostToolUse` cleanup?
- Name a safety hook you'd build *only* when touching prod.
- What's the worst-case infinite loop you could create with hooks?

## Exercises

- **01-basic:** Build three advanced hooks: `PreCompact` snapshot, `Stop` test-runner, directory-freeze safety hook.
- **02-cumulative:** Wire your handoff skill with a `Stop` hook + build a credential-scan safety hook. *Integrates S7, S8.*
