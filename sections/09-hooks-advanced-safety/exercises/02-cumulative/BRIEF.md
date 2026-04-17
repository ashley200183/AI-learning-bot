# Exercise 9.2 — Cumulative: Handoff-on-Stop + Credential Scanner

**Time estimate:** 1.5–2 hours
**Dependencies:** Your S7.1 handoff skill; S8 hook mechanics
**Integrates:** S7 (handoffs), S8 (hooks basics)

## Objective

Two safety-oriented production hooks that use your existing handoff skill and your hook mechanics:

1. **`Stop` hook** that invokes your handoff skill to update `notes/session-handoff.md` before every Stop. Memory survives every session end, automatically.
2. **Credential-scan safety hook** (`PostToolUse` on `Edit|Write`) that scans written file content for leaked secrets. If found, exit 2 with which pattern matched and the file line.

## Why cumulative

S7 gave you handoffs (need to remember to invoke). S8 gave you hooks (always fire). Wiring them means session memory is automatic, not voluntary. And adding credential scanning is a concrete safety use of the hook layer that S7 can't replicate.

## Requirements

### Hook A — auto-handoff on Stop

1. Write `submission/handoff-on-stop.sh` that, on Stop, invokes Claude (via `claude -p` or equivalent) to update `notes/session-handoff.md`. Use your S7.1 skill.
2. Install in `submission/settings.json`.
3. Handle `stop_hook_active` properly (don't loop).
4. Test: work for 10+ minutes, stop. Verify `notes/session-handoff.md` was updated without you asking.

### Hook B — credential scanner

1. Write `submission/cred-scan.sh` that grep-checks the file modified for common secret patterns:
   - AWS access keys: `AKIA[0-9A-Z]{16}`
   - GitHub tokens: `ghp_[A-Za-z0-9]{36}`
   - Private keys: `-----BEGIN (RSA |EC |OPENSSH |)PRIVATE KEY-----`
   - Generic `.env`-style assignments that look like secrets: `(?i)(api[_-]?key|secret|password|token)\s*=\s*['"][A-Za-z0-9+/=_-]{16,}`
2. On match: exit 2 with file path, line number, and matched pattern (but NOT the full match — don't echo the secret into Claude's context).
3. Install as `PostToolUse` on `Edit|Write`.
4. Test: have Claude write a dummy file with a fake `AKIA` string. Verify the block.

## Capture

- `submission/settings.json` — both hooks
- `submission/hooks/handoff-on-stop.sh`
- `submission/hooks/cred-scan.sh`
- `submission/test-results.md` — both tests
- `submission/reflection.md`:
  - Did auto-handoff change your behavior (do you forget to say "wrap up" now)?
  - False-positive risk on the credential scanner — real test files, test fixtures. How did you handle?
  - What's the case for moving the credential scanner to `PreToolUse` (block before write) vs `PostToolUse` (catch after)?

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Auto-handoff fires** — handoff doc updates on Stop | `[GATE]` | /5 | Gate: if the doc doesn't update, fail |
| **Loop guard** — `stop_hook_active` handled | `[GATE]` | /5 | Gate: infinite loop potential = fail |
| **Credential scanner blocks** — on at least two of the listed patterns | `[GATE]` | /5 | Gate: must actually block, not just warn |
| **Secret not echoed** — stderr mentions pattern, not matched text | `[GATE]` | /5 | Gate: if the hook leaks the secret into Claude's context, fail |
| **False-positive handling** — explicit policy | — | /5 | |
| **Pre vs Post argument** — reasoned position | — | /5 | |

## What "good" looks like

Your auto-handoff hook runs `claude -p "Update the handoff doc with what was just done"` silently on Stop. You notice you stopped saying "wrap up" — the hook does it. Your credential scanner has an allowlist for files matching `*.test.*` or paths containing `fixtures/`. Reflection: *"PostToolUse catches the leak before it commits, which is what I care about. PreToolUse would require predicting the write, which is brittle. PostToolUse + the block + Claude's ability to un-write in the same turn is fine."*

## Integration with prior sections (targeted)

- **S7 — load-bearing.** Handoff skill is invoked by the hook.
- **S8 — load-bearing.** Hook mechanics directly applied.
