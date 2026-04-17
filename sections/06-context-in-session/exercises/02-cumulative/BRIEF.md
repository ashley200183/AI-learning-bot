# Exercise 6.2 — Cumulative: `/compact-smart` Command

**Time estimate:** 1.5–2 hours
**Dependencies:** Your S5.2 CLAUDE.md
**Integrates:** S3 (commands), S5 (CLAUDE.md compaction preservation)

## Objective

Build a `/compact-smart` command that invokes `/compact Focus on ...` with the preservation rules from your CLAUDE.md. Instead of typing the focus string every time, the command reads your CLAUDE.md preservation list and injects it.

## Why cumulative

Section 5 told you to put compaction rules in CLAUDE.md. Section 3 taught you to build commands. Wiring them is the natural next step — *and* it forces you to confront that CLAUDE.md alone doesn't guarantee compaction behaves; `/compact Focus on ...` needs the list as an argument.

## Requirements

1. Look at your `submission/project/CLAUDE.md` from Exercise 5.2. Find your compaction-preservation section.
2. Build `submission/compact-smart.md`:
   - `disable-model-invocation: true` (never auto-fire)
   - `argument-hint: <optional-extra-focus>`
   - Body: reads CLAUDE.md's preservation rules, formats them into a `/compact Focus on:` string, optionally appends the user's extra focus.
   - `allowed-tools: Read` (just reading CLAUDE.md)
3. Test in a session that's approaching 60% context. Capture before/after in `submission/test-run.md`:
   - `/cost` before
   - Run `/compact-smart`
   - `/cost` after
   - Prompt Claude something that depends on a preserved decision — verify it still remembers
4. Write `submission/reflection.md`:
   - Did the preservation list survive compaction?
   - Was any item you expected to survive lost?
   - What changed between plain `/compact` and `/compact-smart`?
   - Did you end up iterating your CLAUDE.md's preservation rules based on what you saw?

## Submission

- `submission/compact-smart.md`
- `submission/test-run.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Command wired to CLAUDE.md** — actually reads and uses the preservation section | `[GATE]` | /5 | Gate: hardcoded focus string (no CLAUDE.md read) = fail |
| **Scoped tools** — `allowed-tools: Read` (or defended alternative) | — | /5 | |
| **Real compaction run** — tested near 60%+ context | `[GATE]` | /5 | Gate: synthetic or mocked = fail |
| **Decision-preservation verified** — downstream prompt tested consistency | — | /5 | Prompt after compaction drew on preserved decision |

## What "good" looks like

Your test run shows `/cost` dropping from 160K to 40K tokens. The follow-up prompt asks Claude to "apply the Zod validation rule we agreed on" and Claude does so without re-reading anything. Your reflection notes one item that got lost (a specific file path) and you add it to CLAUDE.md preservation rules.

## Integration with prior sections (targeted)

- **S3 — load-bearing.** Command frontmatter and `allowed-tools` scoping.
- **S5 — load-bearing.** Your CLAUDE.md preservation rules are the data source.
