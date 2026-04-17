# Exercise 16.2 — Cumulative: Real Post-Mortem

**Time estimate:** 1.5–2 hours
**Dependencies:** One prior cumulative exercise that had observable failures (almost all do)
**Integrates:** S5 (CLAUDE.md), S16 (diagnostic framework); plus whichever section you're post-morteming

## Objective

Pick one of your earlier cumulative exercises where something didn't work on the first run. Post-mortem it using the S16 failure-class framework. Produce real changes — a CLAUDE.md rule, a skill edit, or a reusable troubleshooting note.

## Why cumulative

Diagnostic frameworks are sterile until you apply them to your own work. Every cumulative exercise you've done so far probably had *something* that didn't fire, didn't trigger, or missed what you expected. This is where you mine those moments for durable learning.

## Requirements

1. **Choose the target.** Pick one prior cumulative exercise — any of S2.2 through S15.2. In `submission/target.md`:
   - Which exercise
   - What the failure was (in 2–3 sentences)
   - Your initial belief about the cause before this post-mortem
2. **Gather evidence.** Re-read the submission materials for that exercise:
   - Relevant BRIEFs
   - Your run logs
   - GRADE.md feedback (if graded)
   - Any iteration notes
3. **Classify.** Using the S16 framework (trigger / context / instruction / capability), identify the primary failure class. If there were multiple, list them in priority order. → `submission/classification.md`
4. **Root-cause.** For the primary class, drill to the specific mechanism:
   - Trigger: which prompt failed to match which description, and why?
   - Context: what was pushed out of the window, and when?
   - Instruction: what exact wording was ambiguous or contradictory?
   - Capability: what tool or access was missing?
   → `submission/root-cause.md`
5. **Produce fixes** — actual artifacts, not descriptions:
   - CLAUDE.md rule (new or amended)
   - Skill description edit
   - Or a troubleshooting entry in `submission/troubleshooting.md` that captures "if you see X, check Y"
6. **Verify.** Re-run the scenario that failed originally. Confirm the fix helps. Capture in `submission/verification.md`.
7. Write `submission/reflection.md`:
   - Was your initial belief right? If not, what did you miss?
   - Is the fix specific to this exercise or does it generalize? (If generalizable — where else would you apply it?)
   - What S16 diagnostic tool was most useful?

## Submission

- `submission/target.md`
- `submission/classification.md`
- `submission/root-cause.md`
- `submission/troubleshooting.md` (or updated skill/CLAUDE.md)
- `submission/verification.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Real failure from a real cumulative** — not fabricated | `[GATE]` | /5 | Gate: invented failure = fail |
| **Initial belief stated** — honest | — | /5 | Before the post-mortem, not after |
| **Classification correct** | — | /5 | Matches the root cause |
| **Root cause specific** — exact mechanism | `[GATE]` | /5 | Gate: "the skill was weak" = fail. "The description lacked 'migrate' and 'port' as trigger verbs, which is what my prompts used" = pass |
| **Fix is an artifact** — not a description of one | `[GATE]` | /5 | Gate: "I'd update the description" without actually updating = fail |
| **Verification** — re-run shows the fix works | — | /5 | |

## What "good" looks like

Target: S10.2 heartbeat supervisor didn't fire on the staged stall. Initial belief: "the timestamp check was wrong." After post-mortem: no — the timestamp logic was fine. Real root cause: the supervisor ran every 60 seconds, but the heartbeat itself only wrote every 3 minutes. So "stale heartbeat" was the supervisor checking before the heartbeat had written once. Classification: *instruction* (the supervisor's polling interval was set independently from the heartbeat's cadence, implicit rather than coordinated). Fix: new rule — *supervisor polling interval must be ≥ 2× the heartbeat cadence; document both as a pair*. Verification: re-ran, supervisor waited one full heartbeat cycle before first check, no false alarm.

## Integration with prior sections (targeted)

- **S5 — often load-bearing.** Most fixes land as CLAUDE.md rules.
- **S16 — load-bearing.** The diagnostic framework is the method.
- **Whichever section you targeted.** Specific artifacts in that section's submission get updated.
