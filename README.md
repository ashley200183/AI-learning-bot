# AI Learning Bot

An interactive curriculum for learning to build with AI agents, taught by Claude Code itself.

**Two tracks:**
- **Foundations** (optional, 7 sections, `foundations/`) — coding prerequisites for students new to terminals, git, GitHub, project anatomy, and repo strategy. ~7.5–10 hours.
- **AI Curriculum** (19 sections, `sections/`) — the main event. Skills, commands, personas, agents, hooks, heartbeats, orchestration, debugging, evals, MCP, and the Claude Agent SDK. ~90 hours.

Source: `agentic-learning-plan-v3.md` (original Phases 1–7) + four new AI sections (Heartbeats, Debugging, MCP, Agent SDK) + the new Foundations track.

## How it works

- Open this repo in Claude Code. `CLAUDE.md` loads automatically — it turns Claude into your teacher.
- The teacher works through one section at a time: concept discussion (Socratic), check-for-understanding questions, exercise walkthrough, graded submission, and (for sections 2+) a cumulative exercise that integrates specific prior concepts.
- Progress is persisted in `.claude/state/progress.json` and surfaced in `MAP.md`.
- Sessions resume cleanly — the teacher reads `notes/session-handoff.md` and picks up where you left off.

## Commands

- `/resume` — continue where you left off
- `/map` — see the full progress map
- `/grade` — grade the current exercise (via the `grader` subagent with gate-criteria enforcement)
- `/next` — advance to the next section (gated on exercise completion)

## Getting started

Open a Claude Code session in this directory and say: *"Let's begin."*

The teacher will ask first whether you want Foundations or to jump straight to AI Section 1. A quick self-assessment:

- If `git status`, `cd`, creating a GitHub repo, `.env` files, `package.json`, or reading a stack trace feel uncertain → **do Foundations first.**
- If you already use the command line, git, GitHub, and can run + debug your own projects → **skip to AI Section 1.**

You can also say `/resume` to trigger the resume flow explicitly.

## Structure

```
CLAUDE.md                            # teacher's standing orders (both tracks)
MAP.md                               # progress map across both tracks
agentic-learning-plan-v3.md          # original AI curriculum source (Phases 1–7)
.claude/
  skills/teacher/SKILL.md            # teacher persona
  agents/grader.md                   # grading subagent with [GATE] enforcement (AI track only)
  commands/                          # /resume, /map, /grade, /next
  state/progress.json                # canonical progress state for both tracks
foundations/                         # 5 optional prerequisite sections:
  README.md
  NN-<slug>/
    README.md                        # direct explanation
    exercises/01-basic/              # pass/fail exercise
sections/                            # 19 AI sections, each with:
  NN-<slug>/
    README.md                        # lesson notes
    exercises/
      01-basic/                      # BRIEF.md (with [GATE] rubric criteria) + submission/ + GRADE.md
      02-cumulative/                 # (section 2+) names *specific* prior sections it integrates
_starter-project/                    # empty skeleton for cumulative exercises
notes/session-handoff.md             # written at session end, read at session start
```

## Philosophy

Four rules shape everything:

1. **Teach, don't telegraph.** Socratic questions push you to derive the answer. Stuck 2 turns? Direct explanation, then move on.
2. **Gate progress strictly.** Total ≥ 75% AND every `[GATE]` rubric criterion at its gate minimum (default 3/5). A thoughtful reflection cannot compensate for a skill that doesn't trigger.
3. **Cumulative integration is targeted, not compounding.** Each cumulative exercise names the specific prior sections it draws on (typically 2–4) — not every section before it. Keeps rubrics focused and submissions clean.
4. **Exercises live here.** Skills, commands, personas, hooks, heartbeats, eval suites, MCP servers, SDK apps — all built in this repo. At the end you have a reference library of everything you built.

## Extending

- Modify `CLAUDE.md` to change teacher behavior.
- Modify `.claude/skills/teacher/SKILL.md` to change the persona's voice.
- Modify `.claude/agents/grader.md` to change how grades work (gate logic lives here).
- Modify any `sections/NN-<slug>/exercises/<ex>/BRIEF.md` to change what's evaluated.

Do *not* modify `agentic-learning-plan-v3.md` — that's the original curriculum source.
