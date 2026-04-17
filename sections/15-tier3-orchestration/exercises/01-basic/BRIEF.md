# Exercise 15.1 — Schedule a Recurring Tier-3 Task

**Time estimate:** 1.5 hours (plus one day of observation)
**Dependencies:** Claude Code with `CronCreate` support OR Claude Code Web OR equivalent tier-3 tooling

## Objective

Schedule a recurring task that runs without you. Observe one real run. Learn how much prep a brief needs when you can't clarify mid-run.

## Requirements

1. Pick a task that's genuinely recurring and valuable. Good candidates:
   - Daily: summarize overnight CI failures into a digest
   - Daily: check for stale PRs (>7 days) and suggest action
   - Weekly: scan dependency updates, flag breaking changes
   - Every-morning: pull your issue tracker and produce a prioritized list
2. Write the **brief** at `submission/brief.md`. Treat this as a contract:
   - Inputs (what data the agent will have available)
   - Outputs (what it produces, where it writes)
   - Success criteria
   - Failure behavior (what if input is unavailable?)
   - Escalation (if the agent detects something it doesn't know how to handle, what does it do?)
3. Schedule via `CronCreate` or equivalent. Save config in `submission/schedule.md`.
4. **Observe one run.** Capture:
   - Timestamp when it fired
   - Duration
   - Output produced
   - Any failures or warnings
   → `submission/run-1.md`
5. Write `submission/reflection.md`:
   - What did the brief miss that the agent had to guess about?
   - Cost per run — projected monthly if you kept this scheduled?
   - Is this something you'd actually keep scheduled? Or was the exercise the value?

## Submission

- `submission/brief.md`
- `submission/schedule.md`
- `submission/run-1.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Brief is contract-grade** — inputs, outputs, failures, escalation | `[GATE]` | /5 | Gate: missing failure behavior or escalation path = fail. This is the whole point. |
| **Schedule configured and fired** — real run observed | `[GATE]` | /5 | Gate: didn't actually run = fail |
| **Brief-gaps analysis** — specific things it missed | — | /5 | |
| **Keep-or-kill decision** — reasoned | — | /5 | |

## What "good" looks like

Brief has an explicit escalation: *"If GitHub API returns non-200, write the error + timestamp to `notes/tier3-errors.log` and exit — do not retry, do not attempt workarounds."* Your run showed the agent hit a rate limit at 9:03am, wrote to the error log cleanly, and exited. Your reflection notes that without the explicit escalation, it would have tried three creative workarounds unsupervised.

## Integration with prior sections

None explicitly — S10 heartbeat mechanics will come in at 15.2.
