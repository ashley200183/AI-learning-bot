# Exercise 8.1 — First Hook, Then Three

**Time estimate:** 1.5 hours
**Dependencies:** Claude Code CLI; `jq` (for parsing hook payloads — `brew install jq` on macOS)

## Objective

Install ONE hook first in lab-style (just watch it fire). Once you've seen the event payload and understand the mechanics, build two more.

## Lab step — see a hook fire

1. Add this to `.claude/settings.json` in your project (or a scratch dir):
   ```json
   {
     "hooks": {
       "UserPromptSubmit": [{
         "hooks": [{
           "type": "command",
           "command": "echo 'Hook fired at '$(date +%T)' — prompt received' >&2"
         }]
       }]
     }
   }
   ```
2. Submit any prompt. Check Claude Code's debug output (or run with `--verbose`). You should see the hook's stderr line.
3. Save the settings.json + the observation to `submission/lab/`.

**Verify:** if you don't see the hook fire, check that `.claude/settings.json` is valid JSON (run `jq . .claude/settings.json` — should output the parsed file without error).

## Build three production hooks

Create `submission/settings.json` containing:

**Hook A — `UserPromptSubmit`:** inject sprint context from `notes/current-sprint.md`. If the file is missing, inject `"No active sprint."`.

**Hook B — `PreToolUse` matcher `Bash`:** block destructive commands. Must catch at minimum: `rm -rf /`, `DROP TABLE`, `git push --force`. Exit 2 with a clear stderr message that names the specific command and suggests an alternative.

**Hook C — `PostToolUse` matcher `Edit|Write`:** auto-format based on extension:
- `.ts` / `.tsx` / `.js` / `.jsx` → `prettier --write`
- `.py` → `black`
- other → noop (don't crash)

## Test each

1. **A:** create `notes/current-sprint.md` with a single line. Submit any prompt. Verify the content reached Claude (ask Claude *"what sprint are we on?"* — it should know).
2. **B:** ask Claude to run `rm -rf /tmp/test-target`. Verify the block + Claude's acknowledgment.
3. **C:** have Claude edit a `.ts` file with intentionally bad formatting. Verify it's formatted after.

Capture results in `submission/test-results.md`.

## Submission

- `submission/lab/` — lab step output
- `submission/settings.json` — three production hooks
- `submission/test-results.md`
- `submission/reflection.md`:
  - Which hook felt highest leverage?
  - For each of the three: could this have been a skill instead? What would have gone wrong?

## Rubric

Pass: **total ≥ 19 / 25 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Lab step completed** — hook seen firing, payload understood | `[GATE]` | /5 | Gate: skipping lab and going straight to three = fail |
| **Three hooks valid JSON** — loadable | `[GATE]` | /5 | Gate: invalid JSON = fail |
| **Hook B blocks correctly** — exit 2 + useful stderr | `[GATE]` | /5 | Gate: blocks but empty message = fail. Message must name the command and offer alternative. |
| **Each hook tested honestly** | — | /5 | Three real tests, captured output |
| **Skill-vs-hook justification** — per hook | — | /5 | Three specific defenses with failure modes of skill alternative |

## What "good" looks like

Your reflection on Hook B: *"A skill version of this would fire sometimes. A hook version blocks every time. For `rm -rf` the cost of 'sometimes' is unacceptable — one miss destroys data."* Your test shows Claude seeing your stderr *"Blocked: `rm -rf /tmp/test` — use `rm -rf ./tmp/test` (relative path) or narrower target"* and retrying correctly.

## Integration with prior sections

None — this is foundational hooks mechanics.
