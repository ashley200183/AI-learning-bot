# Foundations — Coding Prerequisites for Vibe Coding

**What this is:** A lean 7-section on-ramp for students who need coding fundamentals before entering the main AI curriculum. Covers the minimum to be dangerous: dev environment, command line, git, GitHub, branches/worktrees/environments, project anatomy, and repo strategy.

**What this is NOT:** A computer science course. You'll learn *just enough* to execute the AI curriculum without hitting a wall every time Claude Code says "open a terminal" or "make a branch."

**Who should do this:**
- If you've never used a terminal, do this.
- If you've used git via a GUI but never the command line, do this.
- If you know branches exist but never made one, do this.
- If you've never read a stack trace or don't know what `package.json` is, do this.
- If you're already comfortable with `git`, `gh`, VS Code, running a project locally, and reading error messages — **skip to the AI curriculum** (Section 1).

**Time estimate:** 3–5 hours (some experience) · 7.5–10 hours (true beginner). One afternoon to one weekend.

## Structure

```
foundations/
  01-dev-environment/         # IDE vs terminal, installing VS Code / Claude Code
  02-command-line/            # cd, ls, pipes, installing tools
  03-git-locally/             # init, commit, log, diff, gitignore
  04-github-remotes/          # push, pull, clone, creating a repo
  05-branches-worktrees-envs/ # branching, worktrees, dev/staging/prod
  06-project-anatomy/         # package.json, semver, running code, reading errors
  07-repo-strategy/           # monorepo vs polyrepo, workspace tools
```

## Philosophy

Different tone from the AI curriculum:

- **Direct, not Socratic.** "What is an IDE?" isn't worth a Socratic dance. Explain it, move on.
- **Pass/fail, not graded.** Exercises are "did you do it? does it work?" Show the terminal output or screenshot. No rubric scoring.
- **One exercise per section.** No cumulative exercises — keep it lean.
- **Skip liberally.** If you already know a section's material, take 5 minutes to verify the exercise still runs cleanly, then skip.

## When you're done

The teacher will advance you to the main AI curriculum (`sections/01-mental-models/`).

## File conventions

```
foundations/NN-<slug>/
  README.md              # concept explanation
  exercises/01-basic/
    BRIEF.md             # "do these steps, show it worked"
    submission/          # screenshots, terminal logs, proof it ran
```
