---
name: map
description: Display the full curriculum section map with current progress
argument-hint: (no arguments)
---

Read `.claude/state/progress.json` and display a concise status table of all 12 sections showing:
- Section number + name
- Status: locked / in progress / complete
- Basic exercise: not started / in progress / graded (score) / passed
- Cumulative exercise: N/A (Section 1) or not started / in progress / graded (score) / passed
- Date completed (if applicable)

After the table, print:
- The current section the user is on
- What the immediate next action is
- Total progress as "X / 12 sections complete"

Also read `MAP.md` and verify it matches `progress.json`. If they're out of sync, flag that and offer to rewrite `MAP.md` from the canonical state.
