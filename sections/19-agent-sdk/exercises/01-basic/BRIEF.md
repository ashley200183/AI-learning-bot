# Exercise 19.1 — First SDK Call with Caching + Tool Use

**Time estimate:** 2 hours
**Dependencies:** Python 3.10+ or Node 18+; Anthropic API key (set in env as `ANTHROPIC_API_KEY`); `anthropic` SDK

## Setup

### Python path
```bash
pip install anthropic
export ANTHROPIC_API_KEY=sk-...
```

### Node path
```bash
npm install @anthropic-ai/sdk
export ANTHROPIC_API_KEY=sk-...
```

**Verify:** a quick `anthropic.messages.create` call with model `claude-haiku-4-5-20251001` should return a short response. If it fails, check the key + network + SDK install.

## Objective

Make three SDK calls that progressively exercise prompt caching and tool use. Understand what each primitive actually does.

## Requirements

### Call 1 — plain

A basic call to `claude-haiku-4-5-20251001` with a short user message. Print the response, the usage stats (input tokens, output tokens), and rough cost using current Haiku pricing.

→ `submission/call-1.{py|ts}` + `submission/call-1-output.md`

### Call 2 — with prompt caching

A call with a *long* system prompt (≥2048 tokens — paste in the body of one of your skills from earlier sections, plus a few similar docs to pad). Use the `cache_control` breakpoints per the Anthropic SDK docs.

Make this call *twice* in the same script:
- First call: cache miss (all tokens billed)
- Second call: cache hit (most input tokens billed at ~10%)

Print `cache_read_input_tokens` + `cache_creation_input_tokens` for each call. Calculate cost delta.

→ `submission/call-2.{py|ts}` + `submission/call-2-output.md`

### Call 3 — tool use

Define a tool using JSON schema: `get_current_weather(location: str, unit: 'celsius'|'fahrenheit')`. Use `claude-sonnet-4-6`. Ask: *"What's the weather in Tokyo?"*. Claude will return a `tool_use` block. Your code:
1. Sees the `tool_use`
2. Returns a canned weather response (e.g., `{"temp": 18, "unit": "celsius", "conditions": "rainy"}`) as a `tool_result`
3. Makes a follow-up call with the conversation history + tool_result
4. Claude returns the final natural-language answer

→ `submission/call-3.{py|ts}` + `submission/call-3-output.md`

### Reflection

`submission/reflection.md`:
- Cache ratio you observed in Call 2 — how does it compare to the "~10% on cache hit" rough rule?
- Cost of Call 3's full loop vs what it would cost with all-`claude-sonnet-4-6` (no caching, no tool)? Approximate.
- What's the SDK equivalent of Claude Code's auto-triggering a skill? (Hint: it doesn't exist — *your* code decides.)

## Submission

- `submission/call-1.*` + output
- `submission/call-2.*` + output
- `submission/call-3.*` + output
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 19 / 25 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Call 1 runs, usage/cost reported** | `[GATE]` | /5 | Gate: doesn't run = fail |
| **Call 2: cache hit observed** | `[GATE]` | /5 | Gate: `cache_read_input_tokens > 0` on second call = pass. If not observed, cache wasn't set up correctly — fix. |
| **Call 3: full tool-use loop** | `[GATE]` | /5 | Gate: one-shot tool_use without the follow-up = incomplete loop = fail |
| **Real usage stats captured** | — | /5 | |
| **SDK-vs-Claude-Code translation** — one concrete insight | — | /5 | Named the auto-trigger gap or equivalent |

## What "good" looks like

Call 2 second invocation shows `cache_creation_input_tokens: 0`, `cache_read_input_tokens: 2450`, total cost ~8% of the first call. Call 3's loop: three messages (user question, assistant tool_use, user tool_result, assistant final answer). Reflection: *"The SDK doesn't auto-match skills. My app has to choose which skill-body to include in the system prompt based on its own routing logic. That's the work the Claude Code runtime does for free; in an SDK app it's on me."*

## Integration with prior sections

Translate any prior section's skill/persona into the system prompt for Call 2. Doesn't need to be a specific section — use what you have.
