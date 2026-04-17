# Exercise 7.2 — Cumulative: Handoff-Reviewer Persona

**Time estimate:** 2 hours
**Dependencies:** Your S7.1 handoff skill; your S5.2 CLAUDE.md
**Integrates:** S4 (persona/subagent), S5 (CLAUDE.md), S7 (this section's handoff)

## Objective

Build a handoff-reviewer persona that evaluates *completeness* of a handoff doc before it gets written. Wire it into your CLAUDE.md routing so that "wrap up" triggers handoff → review → rewrite if needed.

## Why cumulative

Section 7.1 made you write handoffs. This forces the question: what makes a handoff *good*? A persona that evaluates completeness (drawing on S4) plus a CLAUDE.md routing rule (drawing on S5) closes the quality loop.

## Requirements

1. Build `submission/handoff-reviewer/SKILL.md` — a persona that, given a draft handoff doc, evaluates:
   - Are decisions captured with rationale?
   - Are open questions specific enough to act on?
   - Are files modified listed completely?
   - What's missing that a fresh session would need?
   Output: structured findings + score.
2. Update your CLAUDE.md (copy `submission/project/CLAUDE.md` from S5.2 as starting point) so the "wrap" flow is: write handoff → run reviewer → if score < threshold, rewrite.
3. Test end-to-end:
   - Start a session with ≥3 decisions.
   - Say "let's wrap."
   - Handoff skill writes draft.
   - Reviewer persona evaluates.
   - Rewrite (if needed) or commit.
4. Capture the full flow in `submission/flow.md`.
5. **Counterfactual:** on another session, skip the reviewer step. Compare the handoff quality. Capture in `submission/without-reviewer.md`.
6. Write `submission/reflection.md`:
   - What does the reviewer catch that self-review (by the same Claude writing the handoff) doesn't?
   - Did the CLAUDE.md routing fire automatically, or did you have to prompt?
   - One thing the reviewer missed?

## Submission

- `submission/handoff-reviewer/SKILL.md`
- `submission/project/CLAUDE.md` (updated)
- `submission/flow.md`
- `submission/without-reviewer.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 19 / 25 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Reviewer evaluates, not summarizes** | `[GATE]` | /5 | Gate: a reviewer that just lists what's there = fail. Must judge completeness. |
| **CLAUDE.md routes the flow** — handoff → review → rewrite | `[GATE]` | /5 | Gate: routing rule absent = fail |
| **End-to-end test** — real, not staged | `[GATE]` | /5 | Gate: ≥3 decisions, real session, observed flow |
| **Counterfactual** — without-reviewer comparison is real | — | /5 | Actually ran it, not hypothesized |
| **Reflection** — specific catches the reviewer made | — | /5 | |

## What "good" looks like

The reviewer catches that your draft handoff said "decided on auth approach" without naming the approach. It scored the handoff 3/10 on specificity. The rewrite says "decided on JWT refresh with rotation; alternatives considered: session cookies (rejected — mobile client), sliding-window tokens (rejected — too complex)." Your counterfactual shows the without-reviewer handoff was 30% shorter but a fresh session asked 4 clarifying questions.

## Integration with prior sections (targeted)

- **S4 — load-bearing.** Persona design carried over.
- **S5 — load-bearing.** CLAUDE.md routing.
- **S7 — load-bearing.** Builds on 7.1's handoff skill.
