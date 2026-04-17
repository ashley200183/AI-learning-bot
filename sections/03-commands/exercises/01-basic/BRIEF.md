# Exercise 3.1 — Build a Custom Command

**Time estimate:** 45–90 minutes
**Dependencies:** Claude Code CLI

## Objective

Build one custom command you'd actually invoke deliberately. Use it twice in real work. Scope its tool access.

## Requirements

1. Pick a deliberate task. Good candidates: `/release-notes`, `/pr-description`, `/update-changelog`, `/deploy-staging`.
2. Build at `submission/<command-name>.md`.
3. Use at least **three frontmatter fields** purposefully: `name`, `description`, plus one of (`allowed-tools`, `argument-hint`, `disable-model-invocation`).
4. Test in real sessions twice. Capture both invocations in `submission/usage-log.md` — the prompt, the output, whether it did what you wanted.
5. Write `submission/reflection.md`:
   - Why a command instead of a skill for this?
   - What `allowed-tools` scoping (if any) and why?
   - Did Claude ever try to auto-run this? What does that tell you?

## Submission

- `submission/<command>.md`
- `submission/usage-log.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Command file** — valid frontmatter, loadable | `[GATE]` | /5 | Gate: must actually install. Broken frontmatter = fail. |
| **Real usage** — two real invocations captured | `[GATE]` | /5 | Gate: staged or contrived uses fail. |
| **Command vs skill justification** — articulated why this is a command | — | /5 | Names the explicit-control argument |
| **Tool scoping** — appropriate `allowed-tools` (or explained why open) | — | /5 | Thoughtful scope + risk it mitigates |

## What "good" looks like

A `/release-notes` command with `allowed-tools: Bash(git log:*), Read` and `argument-hint: <version>`. Reflection explains why this *shouldn't* auto-trigger — you'd get release notes drafted mid-feature.

## Integration with prior sections

None — commands are new mechanics. Section 2.2 cumulative will pair a command with the commit-style skill.
