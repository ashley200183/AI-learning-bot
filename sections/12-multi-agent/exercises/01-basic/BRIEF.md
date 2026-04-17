# Exercise 12.1 — Spawn a Subagent

**Time estimate:** 1 hour
**Dependencies:** Claude Code CLI

## Objective

Hand off a scoped, tool-heavy task to a subagent. Review the output. Notice how your main context stayed clean.

## Requirements

1. Pick a scoped task with a clear "done" state:
   - *"Read `/src/auth/` and produce a token-flow diagram as markdown"*
   - *"Find places where we catch exceptions silently: file + line + class"*
   - *"Run the test suite, categorize failures into flakes vs real, output table"*
2. Spawn a subagent with:
   - Tools: scoped to what's needed (Read, Grep, Bash as appropriate)
   - Clear brief
   - Explicit "done" criterion and return format
3. While it runs, do something unrelated in the main session. (The point is main-context productivity during the fork.)
4. Capture:
   - `submission/brief.md` — what you asked for
   - `submission/agent-output.md` — full return
   - `submission/cost.md` — `/cost` before and after
   - `submission/reflection.md`:
     - Did the subagent stay in scope or drift?
     - How would output differ if you'd done this inline?
     - Minimum context cost of a subagent call?

## Submission

- `submission/brief.md`
- `submission/agent-output.md`
- `submission/cost.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Brief quality** — scoped, tool allowlist, done-state | `[GATE]` | /5 | Gate: missing any of the three = fail |
| **Return preserved** | — | /5 | |
| **Cost measured** — real numbers | `[GATE]` | /5 | Gate: no `/cost` capture = fail |
| **Reflection** — insights, not description | — | /5 | Named a case where inline would have been better |

## What "good" looks like

Brief scoped to `Read, Grep` only. Return is a useful 800-word summary. Main context went up 1.5K tokens total (brief + return). You estimate inline would have been 15K. Reflection notes that the subagent returned a great answer but missed one file because the brief didn't explicitly include `.test.ts` files — an omission you'd have caught inline.

## Integration with prior sections

None explicitly — foundational subagent mechanics. S4 introduced subagent definitions; this exercises them live.
