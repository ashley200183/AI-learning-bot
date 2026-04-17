---
name: resume
description: Resume the curriculum where the user last left off
argument-hint: (no arguments)
---

Read `.claude/state/progress.json` to determine the user's current section, current exercise, and last activity. Then read `notes/session-handoff.md` for context from the previous session.

Greet the user with a one-paragraph summary that includes:
1. What section/exercise they're on
2. What was covered in the last session (from the handoff doc)
3. A proposed next step framed as a question, not a directive

Then wait for the user's confirmation before proceeding. Do not start teaching until they confirm.

If `progress.json` shows they're at the start (no sections completed, no exercise in progress), instead welcome them and ask whether they want to begin Section 1 or get a curriculum overview first.
