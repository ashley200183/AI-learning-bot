---
name: next
description: Advance to the next section — gated on current section completion
argument-hint: (no arguments)
---

Advance the user to the next section, but only if they've earned it.

Steps:

1. Read `.claude/state/progress.json`.
2. Identify the current section.
3. Check the gate:
   - Both exercises (basic + cumulative, where applicable) must be graded **PASS**.
   - The user must explicitly confirm readiness. Ask: *"Ready to move on? You understand what [concept] means and how it connects to the next phase?"*
4. **If the gate is not met:** refuse the advance. Tell the user specifically what's missing:
   - Exercise not started → "The cumulative exercise hasn't been attempted. That's the gate."
   - Exercise failed → "The basic exercise scored 2/4 on the description criterion. Let's iterate on that before advancing."
   - User confidence check failed → "You described commands as 'like skills but manual.' That's half-right. Let's re-visit Phase 2.5 briefly."
5. **If the gate is met:**
   - Mark current section `complete` in `progress.json` with timestamp.
   - Update `MAP.md` to reflect completion.
   - Set next section as `in_progress`.
   - Announce: *"Section N complete. Next up: Section N+1 — [name]. Want to start now or take a break?"*
   - Wait for user to say go before teaching the new section.

Never advance without the gate check. The cumulative exercises in later sections assume earlier concepts are *absorbed*, not just *completed*. Skipping breaks that.
