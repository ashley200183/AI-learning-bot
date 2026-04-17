---
name: grade
description: Grade the current exercise submission using the grader subagent
argument-hint: [optional: section/exercise path, defaults to current]
---

Determine the exercise to grade:
1. If the user provided a path as argument, use that.
2. Otherwise read `.claude/state/progress.json` and use the current in-progress exercise.
3. If no exercise is currently in progress, ask the user which one to grade.

Before spawning the grader:
1. Read the exercise's `BRIEF.md` to confirm the rubric exists.
2. Verify the `submission/` folder has content. If it's empty, tell the user — don't grade empty submissions.
3. For cumulative exercises, locate prior-section submissions that should be integrated.

Spawn the `grader` subagent with:
- Path to `BRIEF.md`
- Path to `submission/`
- For cumulative: paths to relevant prior-section submissions
- Instruction to write results to `GRADE.md` in the same exercise folder

After the grader returns:
1. Read `GRADE.md`.
2. Present the grade to the user in conversation — don't just point at the file.
3. State your own read: do you agree with the grader? If not, say where and why.
4. If it failed, offer one iteration cycle: identify the single highest-leverage change and walk the user through it.
5. If it passed, ask whether the user wants to move to the next exercise or take a break before advancing.

Update `.claude/state/progress.json` to reflect the grade outcome.
