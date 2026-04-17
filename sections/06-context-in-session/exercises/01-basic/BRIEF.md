# Exercise 6.1 — Managed vs Unmanaged (Same Prompts)

**Time estimate:** 60–90 minutes
**Dependencies:** Claude Code CLI

## Objective

Run the *exact same prompt sequence* twice — once unmanaged, once with active context management. Same prompts = clean A/B. Measure token cost, consistency, and speed.

## Requirements

1. Design a prompt sequence in `submission/sequence.md`. Requirements:
   - 10–15 prompts
   - Involves ≥3 file reads (forces tool-result bulk)
   - Has at least one mid-sequence "context check" prompt that *depends on an earlier decision* (e.g., "apply the constraint we agreed on in prompt 3")
2. **Session A (unmanaged):** run the sequence in order. No `/compact`, no `/btw`, no subagents. Capture in `submission/session-a.md`:
   - `/cost` after every 3 prompts
   - Note any case where Claude forgot or contradicted earlier decisions
3. **Session B (managed):** in a fresh session, run the same sequence. This time:
   - Use `/btw` for the context-check prompt instead of asking in-line
   - Run `/compact Focus on [key decisions]` halfway through
   - If any file read is ≥200 lines, say "don't re-read this, summarize what you remember"
   - Capture in `submission/session-b.md`
4. Write `submission/analysis.md`:
   - Final token count for each
   - Number of decision-contradiction instances
   - Which management move had the biggest impact?

## Submission

- `submission/sequence.md` — the prompt list
- `submission/session-a.md`
- `submission/session-b.md`
- `submission/analysis.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Same prompts used both times** — verified | `[GATE]` | /5 | Gate: comparison is invalid if prompts differ. Include diff. |
| **Both sessions run honestly** — real cost checkpoints | `[GATE]` | /5 | Gate: fabricated or backfilled numbers fail. |
| **Decision-dependency prompt included** — tests consistency | — | /5 | Included in the sequence; outcomes compared |
| **Clear takeaway** — named the management move that mattered most | — | /5 | Specific habit to keep |

## What "good" looks like

Same 12 prompts. Session A: ~40K tokens, Claude forgot the "use Prisma not raw SQL" decision on prompt 8. Session B: ~22K tokens, no contradictions, and the `/compact Focus on X` at prompt 7 is what preserved the Prisma rule.

## Integration with prior sections

None — this is foundational context-management mechanics.
