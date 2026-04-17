# Exercise 15.2 — Cumulative: Tier-3 Run with Heartbeat + Auto-Handoff

**Time estimate:** 2.5 hours setup + real run window (up to 2 hours)
**Dependencies:** S10.2 heartbeat primitive; S14.2 tier-2 preparation discipline; cloud tier-3 tooling
**Integrates:** S10 (heartbeats), S14 (tier-2 prep carried over)

## Objective

Run a tier-3 task that *writes a heartbeat every N minutes* and invokes your handoff skill on completion. You are not in the loop during the run — you see only the heartbeat log and the final handoff doc.

## Why cumulative

Tier 3 without observability is flying blind. Heartbeat (S10) gives you liveness. Handoff (S7, via S9.2 auto-handoff) gives you "here's what I did." Together they recreate tier-2 supervision without you having to watch.

## Requirements

1. **Brief the tier-3 agent** with three explicit requirements:
   - Write a heartbeat line to `notes/tier3-heartbeat.log` every 5 minutes in the format from S10.2
   - On completion, write a handoff doc to `notes/tier3-handoff-<timestamp>.md`
   - On any unrecoverable failure, write to `notes/tier3-errors.log` and exit

   Brief goes in `submission/brief.md`.

2. **Pick a task that will legitimately run 30–60 minutes.** Examples:
   - Full codebase audit for a specific pattern (e.g., "find all untyped async functions")
   - Generate comprehensive docs for a module
   - Triage a backlog of 20 issues into categories
3. **Supervise from outside.** While the agent runs, check the heartbeat log every 10 minutes. Record observations in `submission/supervision-log.md`.
4. **On completion:**
   - Read the auto-handoff doc
   - Review the actual output
   - `submission/completion-review.md`: was the handoff accurate? What did it miss?
5. Write `submission/reflection.md`:
   - How often did the heartbeat catch actual progress vs "still alive"? (Was the agent just writing a timer?)
   - Did supervision from outside feel qualitatively different from tier-2?
   - What would you want added to the heartbeat format for tier-3 specifically?

## Submission

- `submission/brief.md`
- `submission/supervision-log.md`
- `submission/completion-review.md`
- `submission/reflection.md`
- Attach `notes/tier3-heartbeat.log` snapshot

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Brief names heartbeat, handoff, error escalation** | `[GATE]` | /5 | Gate: any of the three missing = fail |
| **Real 30-60 min task** | `[GATE]` | /5 | Gate: synthetic or short = fail |
| **Heartbeat log populated** — entries at expected cadence | `[GATE]` | /5 | Gate: no heartbeat log = agent didn't follow brief = fail |
| **Supervision was real** — logged checks | — | /5 | |
| **Completion review** — honest delta between handoff and reality | `[GATE]` | /5 | Gate: handoff matches reality without checking = fail (suspicious) |
| **Heartbeat improvement proposal** — specific addition for tier-3 | — | /5 | |

## What "good" looks like

Heartbeat log shows 11 entries over 55 minutes. Entry 6 shows the agent stuck on the same "still scanning module X" for 15 minutes — caught by your supervision, you intervened and updated the brief mid-run (tier-3 tooling may or may not allow this; if not, you'd have killed and restarted). Auto-handoff doc had a gap: it didn't mention that 3 issues got categorized twice due to overlapping tags. Your reflection proposes adding a *work-item count* field to the heartbeat: progress from "12 issues triaged" → "15 issues triaged" shows real progress vs a liveness-only heartbeat.

## Integration with prior sections (targeted)

- **S10 — load-bearing.** Heartbeat primitive is the supervision mechanism.
- **S14 — load-bearing.** Prep discipline (brief as contract) carries over directly.
