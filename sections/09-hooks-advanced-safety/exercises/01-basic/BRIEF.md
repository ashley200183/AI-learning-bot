# Exercise 9.1 — Three Advanced Hooks

**Time estimate:** 2 hours
**Dependencies:** A project with a test suite (or willingness to write a quick one); `jq`

## Objective

Build three advanced hooks: a `PreCompact` snapshot, a `Stop` test-runner, and a directory-freeze safety hook. Each serves a different purpose; together they show the range.

## Requirements

### Hook A — `PreCompact` snapshot

Before compaction fires, copy the current `notes/session-handoff.md` (or current planning doc) to a timestamped snapshot file. This is your save-point against lossy compaction.

```bash
# Example implementation approach
cp notes/session-handoff.md .claude/snapshots/pre-compact-$(date +%Y%m%d-%H%M).md
```

### Hook B — `Stop` test-runner

When Claude stops (says done), run the project's test suite. If it fails, exit 2 with the failure output. If tests pass, exit 0.

**Critical:** guard against infinite loops. Read the `stop_hook_active` flag from the hook payload. If it's already true, exit 0 — Claude is stopping *because* the hook already fired once and failed.

### Hook C — Directory freeze safety hook

`PreToolUse` on `Edit|Write`. Read the target file path from `$CLAUDE_FILE_PATHS`. If the path is outside the project's allowed directories (configurable via a file like `.claude/allowed-paths.txt`), exit 2 with a clear message.

**Verify:** ask Claude to edit a file outside the allowed paths. The block must fire with a message Claude can act on.

## Test each

- **A:** Trigger compaction (either manually with `/compact` or by filling context). Confirm a snapshot file appeared.
- **B:** Intentionally break a test. Have Claude do anything, then stop. Verify exit 2 + Claude seeing the failure + Claude continuing to fix.
- **C:** Ask Claude to edit `/etc/hosts` (or any path outside your allowed list). Verify the block.

Capture all tests in `submission/test-results.md`.

## Submission

- `submission/settings.json`
- `submission/hooks/` — any shell scripts
- `submission/allowed-paths.txt`
- `submission/test-results.md`
- `submission/reflection.md`:
  - Hook B has a loop risk. How did you guard it?
  - Hook C blocks legitimate work if allowed-paths is wrong. What's your iteration plan?
  - Which of these three would you turn on for all your projects, and which are project-specific?

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **`PreCompact` hook** — fires before compaction, saves state | — | /5 | Verified snapshot appears |
| **`Stop` hook** — runs tests, blocks on failure | `[GATE]` | /5 | Gate: must correctly handle `stop_hook_active` to avoid loops |
| **Directory-freeze hook** — blocks edits outside allowed paths | `[GATE]` | /5 | Gate: blocks correctly + useful stderr |
| **Tests captured** — each hook exercised | — | /5 | |
| **Loop-risk reasoning** — named the guard + failure mode | `[GATE]` | /5 | Gate: "I set stop_hook_active and it works" without naming the mechanism = fail |
| **Turn-on-always vs project-specific** — reasoned positions | — | /5 | |

## What "good" looks like

Your Hook B reads the JSON payload with `jq`, exits 0 immediately if `.stop_hook_active == true`, and otherwise runs `npm test` piped through `tail -20` into stderr if it fails. Your Hook C reads allowed-paths and compares using a `realpath` check (not string matching, which is bypassable). Your reflection notes that Hook A is universal, Hook B requires a reliable test suite (project-specific), Hook C requires discipline around allowed-paths maintenance.

## Integration with prior sections

None — hooks-advanced mechanics extending S8.
