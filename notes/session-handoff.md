# Session handoff

_Read at every session start. Updated at every session end. Single source of cross-session memory._

## Most recent session

**Date:** 2026-04-16 (foundations expanded to 7 sections)
**Track:** not yet chosen — user has not started either curriculum
**Current position:** foundations/01-dev-environment (unlocked by default)

## What was covered this session

- Built full 19-section AI curriculum scaffold with `[GATE]` rubric criteria, targeted integration rules, grader subagent, starter project template
- Added 5-section Foundations track: dev environment, command line, git locally, GitHub & remotes, branches/worktrees/environments
- **Expanded Foundations to 7 sections** based on gap analysis:
  - F6 — Project Anatomy & Running Code (config files, semver, running projects, reading stack traces) — highest-ROI addition; closes gaps that would have bitten in AI Sections 9, 16, 17
  - F7 — Repo Strategy: Mono vs Poly (workspace tools, directory conventions, worktree behavior in monorepos) — addresses user's specific question
- Wired foundations into progress.json, MAP.md, CLAUDE.md, README.md

## Decisions made

- **Two-track structure.** Foundations is opt-in. First session asks the user which to start with.
- **Foundations teaching style:** direct, not Socratic. These are concrete facts.
- **Foundations grading:** pass/fail verification, not rubric + gates. Teacher checks submission against BRIEF's "Pass criteria" checklist; no grader subagent.
- **AI Curriculum rules unchanged:** Socratic-with-scaffolding, strict gate, targeted cumulative integration.
- **Foundations covers:** dev env, CLI, git, GitHub, branches/worktrees/envs, project anatomy (package.json, semver, running code, stack traces), repo strategy (mono vs poly).
- **Foundations does NOT cover:** deep testing pedagogy, framework-specific content, deep deployment, databases, web fundamentals (HTTP/REST), programming languages as such. Students pick these up as needed.

## Session-start self-assessment (ask the user on first session)

*"Before we start — do you want to do Foundations first or skip to AI Section 1? Foundations covers dev environment, command line, git, GitHub, branches, worktrees, project anatomy, and repo strategy — ~7.5–10 hours. Skip it if `git status`, `cd`, creating a GitHub repo, `.env` files, `package.json`, and reading a stack trace all feel comfortable. Not sure? Do Foundations. It's a weekend."*

Record the answer in `progress.json.foundations.opt_in` (true/false).

## What's next

Depends on user's opt-in answer:
- **Foundations chosen:** start F1 — Your Dev Environment. Read `foundations/01-dev-environment/README.md`, help them install VS Code, Claude Code, git, and run F1.1 exercise.
- **Skipped:** start AI Section 1 — Mental Models. Read `sections/01-mental-models/README.md` and begin Socratic teaching.

## Files modified this session

- `foundations/` — 7 sections total; F6 and F7 newly added
- `CLAUDE.md` — updated section counts + self-assessment heuristic
- `MAP.md` — 7 foundations entries
- `README.md` — updated section counts
- `.claude/state/progress.json` — F6, F7 added to foundations block
- `foundations/README.md` — updated structure listing

## Total curriculum time (updated)

- Foundations: 7.5–10h (was 5–7h with 5 sections)
- AI Curriculum: ~90h
- **Grand total with Foundations: ~97–125h**

## Open questions

None.
