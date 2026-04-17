# Section 15 — Tier 3: Cloud Async Orchestration

**Source:** `agentic-learning-plan-v3.md` → Phase 6 (part B)
**Prerequisites:** Sections 1–14 complete
**Estimated time:** 2.5 hours

## What you'll walk away knowing

- Tier 3 = agents running in cloud VMs or managed environments (Claude Code Web, API-side orchestration). You assign, walk away, check back.
- Scheduled / triggered execution via `CronCreate` + remote triggers
- The handoff semantics that matter when you're not in the loop
- When tier 3 beats tier 2 (overnight backlogs, recurring tasks, low-supervision parallel work)

## Concepts the teacher will work through with you

1. **Tier 2 is local parallel with you watching. Tier 3 is cloud parallel without you.** The failure modes shift.
2. **Briefs must be bulletproof.** You can't clarify mid-run. The brief is the contract.
3. **Heartbeats matter more.** S10's heartbeat primitive is the difference between "agent ran for 3 hours and finished" and "agent silently crashed at minute 8."
4. **Output review becomes the whole job.** You don't code during tier-3 runs; you review their outputs.

## Check-for-understanding questions

- Give one task that's *great* for tier 3 and one that's terrible.
- What changes in the agent brief when you're not available to clarify?
- Why does the heartbeat from S10 become critical here, not just nice-to-have?
- What's the blast radius of a tier-3 agent that goes rogue vs a tier-2 agent?

## Exercises

- **01-basic:** Schedule one recurring tier-3 task using `CronCreate` or equivalent. Observe one run.
- **02-cumulative:** Tier-3 run with heartbeat monitoring + auto-handoff. *Integrates S10, S14.*
