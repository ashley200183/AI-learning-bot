# Exercise 11.1 — Deep-Research Fork

**Time estimate:** 1 hour
**Dependencies:** Claude Code CLI (supports `context: fork`)

## Objective

Build a forked skill for codebase research. Use it on a module you don't know well. Compare main-context cost to doing the research inline.

## Requirements

1. Build `submission/deep-research/SKILL.md` with `context: fork` and `agent: Explore` frontmatter. Body should include a research methodology: find relevant files (Glob, Grep), read and analyze, summarize with file references.
2. Pick a module in a codebase you know but haven't recently dived into.
3. **Baseline:** `/cost` in your main session. Invoke `deep-research` on the module. Capture `/cost` after — main context should have barely moved.
4. **Inline:** in a fresh session, do the same research inline (read files directly, ask Claude to analyze). `/cost` after.
5. Write `submission/comparison.md`:
   - Token cost: main-session after fork vs inline
   - Summary quality side-by-side (fork's return vs what you learned inline)
   - What did inline catch that fork missed (if anything)?
6. Write `submission/reflection.md`:
   - For what task types would you always fork?
   - For what would you never fork?
   - Where does the fork's summary hide information you might need later?

## Submission

- `submission/deep-research/SKILL.md`
- `submission/comparison.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Skill valid** — `context: fork` + `agent: Explore` correct | `[GATE]` | /5 | Gate: wrong frontmatter = doesn't actually fork = fail |
| **Cost comparison real** — both sessions actually run | `[GATE]` | /5 | Gate: fabricated numbers = fail |
| **Summary quality compared** — side-by-side, honest | — | /5 | |
| **When-to-fork heuristic** — actionable | — | /5 | Rule + named exception |

## What "good" looks like

Your main context moves ~600 tokens (the summary) when fork runs. Inline, same research costs ~12K. Fork missed one edge case that inline caught because inline could ask follow-ups. Reflection: *"Fork for initial orientation on any unfamiliar module. Inline when I need to argue with the findings."*

## Integration with prior sections

None — introduces forking mechanics.
