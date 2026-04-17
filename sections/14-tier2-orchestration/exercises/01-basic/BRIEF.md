# Exercise 14.1 — `plan` Mode

**Time estimate:** 1–1.5 hours
**Dependencies:** oh-my-claudecode (see setup below)

## Setup — install oh-my-claudecode

Per the project's README. Typically:
```bash
git clone https://github.com/<org>/oh-my-claudecode.git ~/.claude/plugins/oh-my-claudecode
# Follow its setup instructions for registering the commands
```

**Verify:** run `/plan` — you should see the planning interview start. If not recognized, check the plugin's install logs for errors and confirm the commands registered in your Claude Code commands list.

## Objective

Run `plan` mode on a feature you're about to build. Note every scope issue surfaced that you'd otherwise have hit in code.

## Requirements

1. Pick a real feature with real ambiguity. Good candidates:
   - *"Add a webhook handler for payments"*
   - *"Add search to the settings page"*
   - *"Migrate to a new auth provider"*
   At least one decision must be genuinely unmade going in.
2. Run `plan` mode. Answer its questions honestly — don't tidy up answers after the fact. Save the session to `submission/plan-session.md`.
3. Write `submission/surfaced-issues.md` — for each scope issue plan mode raised:
   - The question
   - Your honest initial answer (or "I hadn't decided")
   - What would have happened if you'd hit it in code without planning
4. Write `submission/reflection.md`:
   - How many issues would you have caught without plan mode?
   - Which question surprised you?
   - What issues do you think plan mode should have surfaced but didn't?

## Submission

- `submission/plan-session.md`
- `submission/surfaced-issues.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Real feature with ambiguity** | `[GATE]` | /5 | Gate: pre-decided feature = no value = fail |
| **Plan session completed honestly** | `[GATE]` | /5 | Gate: half-complete or sanitized answers = fail |
| **Surfaced issues with counterfactuals** — specific, named | — | /5 | Each issue paired with "what breaks in code" |
| **Honest critique** — what plan mode missed | — | /5 | |

## What "good" looks like

Plan mode asked *"What payment events specifically? Disputes? Refunds? Chargebacks?"* — you'd only thought about successful charges. Counterfactual: you'd have hit dispute-handling halfway through implementation, realized the data flow didn't support it, and either rewritten or ignored disputes. Your critique: *"Plan mode didn't ask about idempotency. For webhooks that's the second biggest scope question. Should have."*

## Integration with prior sections

Your S5 CLAUDE.md matters here — plan mode reads it. A weak CLAUDE.md produces a weak plan.
