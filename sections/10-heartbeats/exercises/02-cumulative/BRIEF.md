# Exercise 10.2 — Cumulative: Heartbeat + Stall Escalation

**Time estimate:** 2 hours
**Dependencies:** Your S7.1 handoff skill; your S9.2 auto-handoff hook; S8 hook mechanics; S10.1 loop skill
**Integrates:** S7 (handoffs), S8 (hooks), S9 (advanced hooks), S10 (this section)

## Objective

Build a heartbeat primitive for long-running agents: every N minutes during a long task, write a structured status line to `notes/heartbeat.log`. A supervisor (you, or a second agent) watches the log. If the last heartbeat is more than 2N minutes old, escalate.

## Why cumulative

This is the first real temporal system. Heartbeats alone tell you an agent is alive. Paired with handoffs (S7), they capture what the agent is doing. Paired with a stall-detection `Stop` hook (S9), they escalate when stuck. Paired with the `/loop` skill (S10.1), they run autonomously.

## Requirements

### Part A — heartbeat writer

1. Build `submission/heartbeat/SKILL.md`. When invoked, it writes a line to `notes/heartbeat.log`:
   ```
   2026-04-16T14:32:05Z | section:02-skills ex:01-basic | status:in-progress | next:iterating on description
   ```
2. Trigger conditions: during long-running work, when agent crosses a task boundary, or on every N-th `/loop` cycle.

### Part B — heartbeat supervisor

1. Build a second skill or a small standalone script at `submission/supervisor/`. Given the heartbeat log, it:
   - Reads the last line
   - Checks timestamp vs now
   - If gap > threshold (default 10 min), emit a stall alert
2. You decide the form: a shell script cron'd, another `/loop` skill, or a `Stop` hook that checks on every stop.

### Part C — real scenario

Run an actual long task for 30+ minutes (any real task from your work). During it:
- The heartbeat writes every N minutes (you pick N, defend the choice).
- At one point, intentionally stall the agent for > threshold to test escalation.
- Supervisor must detect the stall.

Capture all of this in `submission/run/`:
- `run/task-description.md` — what was being worked on
- `run/heartbeat.log` — actual log
- `run/supervisor-alerts.md` — the stall escalation that fired
- `run/transcript-excerpts.md` — key moments

### Part D — reflection

`submission/reflection.md`:
- Heartbeat cadence — what did you pick, and why? What tradeoffs?
- How is heartbeat different from your S7 handoff doc?
- How is heartbeat different from a `Stop` hook that runs tests? (Hint: time-based vs event-based.)
- What's the failure mode of a heartbeat that *always* succeeds even when the agent is confused?

## Submission

- `submission/heartbeat/SKILL.md`
- `submission/supervisor/` — script or skill
- `submission/run/` — evidence of a real 30+ min scenario
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Heartbeat writes valid lines** — parseable, timestamped | `[GATE]` | /5 | Gate: unparseable log = supervisor can't work = fail |
| **Supervisor detects stalls** — actually fired in real test | `[GATE]` | /5 | Gate: no stall detected in the staged stall = fail |
| **30+ min real scenario** — not synthetic | `[GATE]` | /5 | Gate: fabricated timestamps = fail |
| **Cadence reasoning** — defended N choice | — | /5 | |
| **Heartbeat-vs-handoff analysis** — named the difference | — | /5 | Heartbeat = "am I alive." Handoff = "here's what I did." Both matter. |
| **Silent-success failure mode** — named | — | /5 | Heartbeat that always succeeds even when lost = anti-pattern. Named and proposed mitigation. |

## What "good" looks like

You pick 3-minute heartbeats. On the staged stall, supervisor alerts at minute 11 — 3× threshold for clarity. Your reflection nails the silent-success problem: *"A heartbeat that just writes `status:alive` is worthless. The line must include what the agent *thinks* it's doing — if that stays the same for 3 heartbeats, that's a different kind of stall even though the timer is fresh."* You propose including a checksum of the current file-being-worked-on as part of the line.

## Integration with prior sections (targeted)

- **S7 — load-bearing.** Contrast handoff vs heartbeat; supervisor might invoke the handoff skill.
- **S8 — load-bearing.** Hook mechanics if your supervisor is a `Stop` hook.
- **S9 — load-bearing.** Stall-detection hook borrows loop-guard patterns from 9.1.
- **S10 — load-bearing.** `/loop` skill from 10.1 can be the heartbeat's engine.
