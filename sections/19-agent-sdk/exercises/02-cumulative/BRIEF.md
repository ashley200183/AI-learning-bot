# Exercise 19.2 — Cumulative: Port a Workflow to the SDK

**Time estimate:** 4–5 hours (this is the grand capstone)
**Dependencies:** Anthropic SDK; prior-section artifacts you'll port
**Integrates:** Whichever workflow you port + S17 (eval suite discipline)

## Objective

Take one workflow you built in Claude Code and port it to an SDK app. Make the translation explicit. Measure cost. Ship a small working thing.

## Pick the workflow

Any of:
- **S4.2 review-and-commit** — persona + subagent + skill + command
- **S7.2 handoff + reviewer** — session-end workflow with quality gate
- **S12.2 three-reviewer PR team** — parallel subagents with synthesis
- **S13.2 batch-migration** — worker agents in isolation

Pick based on what you most want to ship. Document choice in `submission/choice.md`.

## Requirements

### Part A — port

Build at `submission/app/` an SDK-based implementation. Structure suggestion:

```
app/
  main.{py|ts}           ← entry point
  system_prompts/
    base.md              ← CLAUDE.md equivalent
    reviewer.md          ← persona equivalent
    ...
  tools/
    <tool>.{py|ts}       ← tool implementations
  routing.{py|ts}        ← decides which system prompt / model per request
  run.{py|ts}            ← the workflow loop
```

Requirements:
- **Prompt caching** on the base system prompt
- **At least one tool** defined via SDK tool-use
- **Model routing** — at least two different models used (justify in reflection)
- **Structured output** — final result written to a file or returned in a predictable shape

### Part B — use eval suite discipline (from S17)

Build 5 test cases at `submission/app/cases/` (input + expected properties). Run the ported workflow on each; score. Compare to running the Claude Code version of the same workflow on the same 5 cases.

→ `submission/app/results.md`

### Part C — cost comparison

`submission/cost-comparison.md`:
- Cost per run of the Claude Code version (estimate based on model + tokens from your logs)
- Cost per run of the SDK port
- Ratio
- Where the savings came from (caching? routing? simpler loop?)

### Part D — mapping table

`submission/mapping.md` — a filled-out version of the translation table from the README, using YOUR specific artifacts:

| Claude Code concept | In your ported app |
|---|---|
| CLAUDE.md rule "X" | system_prompts/base.md line Y |
| skill Z | system_prompts/Z.md, included when routing detects [condition] |
| command `/foo` | app endpoint / CLI arg `--foo` |
| subagent "reviewer" | separate API call with system_prompts/reviewer.md + tools [...] |
| hook "auto-format" | middleware function `after_tool_use()` |

### Part E — reflection

`submission/reflection.md`:
- What was the hardest part of the port?
- What did Claude Code do for free that you had to build?
- What was cheaper / better in the SDK version?
- If you had to ship this to production, what's the gap between what you built and shippable?
- Would you port more workflows? Which next?

## Submission

- `submission/choice.md`
- `submission/app/` — full code
- `submission/app/results.md` — eval suite results
- `submission/cost-comparison.md`
- `submission/mapping.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 30 / 40 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **App runs end-to-end** | `[GATE]` | /5 | Gate: broken main.{py/ts} = fail |
| **Prompt caching in use** | `[GATE]` | /5 | Gate: no caching = you learned nothing from 19.1 = fail |
| **Tool use loop works** | `[GATE]` | /5 | Gate: multi-turn loop with real tool_result = required |
| **Model routing real** — ≥2 models, justified | `[GATE]` | /5 | Gate: all-Opus = fail (wasted); all-Haiku = fail (quality gap) |
| **5 eval cases run against both versions** | — | /5 | |
| **Cost comparison with real numbers** | — | /5 | |
| **Mapping table complete and specific** | — | /5 | Not generic — names your actual files |
| **Production-gap honesty** | — | /5 | What's missing for shippable |

## What "good" looks like

You ported S12.2's three-reviewer team. `main.py` accepts a diff as stdin. `routing.py` classifies which reviewers to spawn based on the diff (always security + test; performance only if >3 files). `tools/` has `get_diff_stats`, `run_lint`, `run_tests`. Base system prompt (4K tokens) is cached; reviewer-specific prompts are appended per call. Security uses Opus (stakes high, rare); test uses Sonnet (volume work); classification uses Haiku. Eval on 5 cases shows the SDK version catches 4/5 issues the Claude Code version caught, and added 1 issue via a tool call that Claude Code couldn't do. Cost: SDK version is $0.34/run (Claude Code version: ~$0.80 equivalent). Mapping table names specific files. Production-gap reflection: *"Need auth, queuing, observability, retry logic. What I built is a runnable prototype, not a service."*

## Integration with prior sections (targeted)

- **The workflow you ported.** Load-bearing — the artifacts must come from that section's submission, not invented for this exercise.
- **S17 — load-bearing.** Eval suite discipline applied to the port.
