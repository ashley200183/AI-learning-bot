# Section 10 — Heartbeats & Temporal Triggers

**Source:** NEW — not covered in the original plan
**Prerequisites:** Sections 1–9 complete
**Estimated time:** 2 hours

## What you'll walk away knowing

- Two flavors of temporal triggering: **scheduled** (cron-style, fires on a clock) and **heartbeat** (agent pings at regular intervals to prove it's alive)
- Claude Code's `/loop` (self-paced or interval-based), `CronCreate` (scheduled triggers), `ScheduleWakeup` (dynamic self-scheduled wakeups)
- Heartbeat patterns: long-running agents that report state periodically; escalation on stall
- Time-bounded agents — how to cap runtime so a stuck agent can't burn budget forever

## Why this section exists

Before you put Claude on a cron or let it run unattended in tier-2 orchestration (Section 14), you need to know how to time-bound, heartbeat, and escalate. Skipping this is how agents silently fail for hours.

## Concepts the teacher will work through with you

1. **Scheduled vs heartbeat.** Scheduled = "run at 9am daily." Heartbeat = "prove you're alive every 5 minutes while working." Different problems.
2. **`/loop` modes.**
   - With an interval: `/loop 5m /check-status` — fires every 5 minutes.
   - Without an interval: Claude self-paces, decides when to fire next based on task.
3. **`CronCreate` and remote triggers.** Scheduled agents that can run even when your session is closed (API-side orchestration).
4. **`ScheduleWakeup` dynamic cadence.** For self-paced loops: Claude chooses its own wake-up interval based on what it's watching for. Cache-window-aware (≤270s keeps the 5-min prompt cache warm; past 300s pays a cache miss).
5. **Heartbeat as an observability primitive.** A loop that writes a one-line status to a file every N minutes. Supervisors (human or agent) watch the file; absence of update = stall.
6. **Time-bounded agents.** Max runtime budgets enforced by a `Stop` hook (from S9) or by a heartbeat-missed escalation.

## Check-for-understanding questions

- Scheduled vs heartbeat — give one real use case for each.
- You have an agent that's supposed to work for an hour but keeps ending in 5 minutes. What's the temporal primitive that fixes that?
- What's the risk of `/loop 60s` vs `/loop 10m`? How do you pick?
- Why does `ScheduleWakeup` care about 300 seconds specifically?

## Exercises

- **01-basic:** Build a self-paced `/loop` skill that polls for a condition and exits.
- **02-cumulative:** Heartbeat skill for long-running agents, wired with your S9 auto-handoff. *Integrates S7, S8, S9.*
