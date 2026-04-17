# Exercise 7.1 — Handoff Protocol

**Time estimate:** 1.5 hours
**Dependencies:** Claude Code CLI

## Objective

Build a handoff skill + command. Test it by working, `/clear`ing, and continuing from the handoff doc alone.

## Requirements

1. Build `submission/handoff-writer/SKILL.md`. Triggers on session-end signals ("let's wrap", "stopping for today", "end session") AND on explicit invocation. Body instructs Claude to produce a structured handoff doc with sections: *What was done, Decisions made, Files modified, Open questions, What's next*.
2. Build `submission/handoff.md` command to manually invoke the skill and write to `notes/session-handoff.md`.
3. **Test the round-trip:**
   - Start a session on any non-trivial task. Make ≥3 decisions with rationale.
   - Say "let's wrap" — verify the skill fires.
   - Check `notes/session-handoff.md` — does it capture the decisions?
   - `/clear`
   - In the fresh session, read the handoff doc and continue.
   - Does the fresh session ask questions the handoff didn't cover? Note them.
4. Capture the full round-trip in `submission/round-trip.md`.
5. Write `submission/reflection.md`:
   - What did the handoff capture well? What did it miss?
   - If you could add one section to the handoff format, what?
   - Did the fresh session ask anything that revealed a gap?

## Submission

- `submission/handoff-writer/SKILL.md`
- `submission/handoff.md` (command)
- `submission/round-trip.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Handoff skill** — triggers on end-signals AND explicit invocation | `[GATE]` | /5 | Gate: if only one trigger path works, fail |
| **Handoff command** — valid, scoped, writes to target | — | /5 | |
| **Real round-trip** — ≥3 decisions, `/clear`, continued successfully | `[GATE]` | /5 | Gate: trivial task or staged round-trip = fail |
| **Gap identification** — specific things the handoff missed | — | /5 | Not "went fine" — named gaps |

## What "good" looks like

Your round-trip doc shows the fresh session asking "why did we pick Zod over Yup?" — revealing the handoff captured decisions but not rationale. Your reflection's proposed addition is "Rationale per decision," and you update the skill accordingly.

## Integration with prior sections

None — this is foundational handoff mechanics.
