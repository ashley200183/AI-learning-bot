# Exercise 10.1 — Self-Paced Polling Loop

**Time estimate:** 1 hour
**Dependencies:** Claude Code CLI (supports `/loop`)

## Objective

Build a skill that uses `/loop` (dynamic-pacing mode via `ScheduleWakeup`) to poll for a real condition and exit when met. Learn the cadence trade-offs firsthand.

## Concrete scenario

Pick one of these (or an equivalent from your own work):
- Poll a build log until "Build succeeded" or "Build failed" appears.
- Poll a deployed URL until it returns HTTP 200.
- Watch a file for a specific line to appear.
- Monitor a git branch for a new commit.

## Requirements

1. Build `submission/poll-<thing>/SKILL.md`. Description should reference `/loop` and the specific polling scenario.
2. The body must:
   - Describe the exit condition clearly
   - Specify cadence reasoning — why N seconds? Reference the 270s / 300s cache-window rule.
   - Handle the "not met yet" case — `ScheduleWakeup` back with chosen delay
   - Handle the "met" case — report result, do not re-schedule
   - Handle the "timed out after X attempts" case — escalate, don't silently loop forever
3. **Actually run it.** Kick off the scenario, invoke the skill, let it run to natural completion.
4. Capture in `submission/run-log.md`:
   - Each wake-up: timestamp, what was checked, what was found, next delay chosen
   - Total wall-clock time
   - Total approximate token cost (note: each wake-up is a fresh turn)
5. Write `submission/reflection.md`:
   - Did Claude pick reasonable delays, or did you need to override?
   - What would have gone wrong with `/loop 30s` (too aggressive) or `/loop 30m` (too slow)?
   - Would you switch to a cron-style `CronCreate` for this, or keep it dynamic?

## Submission

- `submission/poll-<thing>/SKILL.md`
- `submission/run-log.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Skill valid and runs** — `/loop` fires correctly | `[GATE]` | /5 | Gate: if it never fires, fail |
| **Exit condition handled** — loop terminates when met | `[GATE]` | /5 | Gate: infinite loop or wrong exit = fail |
| **Timeout handled** — not left running forever | `[GATE]` | /5 | Gate: no escape hatch = fail |
| **Cadence reasoning** — cache-window awareness | — | /5 | References 270s / 300s threshold |

## What "good" looks like

Your run-log shows 6 wake-ups over 20 minutes: first three at 60s (build just started, quick turnaround), then two at 270s (build deep in the middle — don't burn cache for nothing), last one at 90s (build nearing completion per timing signals). Exit condition hits on wake-up 6. Reflection: *"Dynamic pacing matched reality better than a fixed cron could — 60s when useful, 270s when wasteful."*

## Integration with prior sections

None — introduces temporal mechanics.
