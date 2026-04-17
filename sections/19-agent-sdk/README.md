# Section 19 — Claude Agent SDK / API

**Source:** NEW — programmatic access to Claude, not Claude Code
**Prerequisites:** Sections 1–18 complete
**Estimated time:** 4–5 hours

## What you'll walk away knowing

- The shift from Claude Code (interactive) to programmatic (`anthropic` SDK, Claude Agent SDK, Managed Agents API)
- How the primitives you've learned *translate* — skills → system prompts + prompt caching, personas → system prompts, subagents → separate API calls, hooks → your own middleware
- When to build *on the API* vs stay in Claude Code
- Prompt caching — the cost / performance lever for production systems
- Tool use via the API — JSON schema declarations, multi-turn tool-use loops

## Why this section exists — the real goal

"Fully customised AI systems" often means building your own application that uses Claude as an engine. Claude Code gives you the interactive environment to learn. The SDK gives you the building blocks to ship. Every primitive in Sections 1–18 has an SDK equivalent; this section makes the translation explicit and has you build one small production-shape app.

## Concepts the teacher will work through with you

1. **The translation table:**
   | Claude Code concept | SDK equivalent |
   |---|---|
   | CLAUDE.md | `system` prompt (persistent, cache-able) |
   | Skills (description + body) | Cached system-prompt sections chosen by your app's routing |
   | Commands | Your app's endpoints — you control when they fire |
   | Personas | System-prompt prefixes, often model-routed (Opus for high-stakes personas) |
   | Subagents | Separate API calls with scoped system prompts + tool allowlists |
   | Hooks | Middleware you write around the API call |
   | MCP servers | Actual MCP — the SDK supports MCP clients natively; or tool declarations for API-native tools |
   | Forks | Separate conversation threads in your app |
2. **Prompt caching.** Long system prompts get cached; subsequent calls pay ~10% of the tokens. For production, this is the difference between viable and expensive.
3. **Tool use loop.** Request → assistant response with tool_use block → your app executes → tool_result → next request. The SDK gives you the primitives; you own the loop.
4. **Managed Agents API.** If your app *is* an agent (not just a Claude call), the Managed Agents API handles the loop for you.

## Check-for-understanding questions

- Your Claude Code skill triggers automatically. Your SDK app must *decide* when to include a skill's content. What replaces the auto-trigger?
- Your app has a 12K-token system prompt. One request costs $X without caching, $Y with. Estimate the ratio.
- When is it right to stay in Claude Code vs build your own SDK app?
- You want a tool that fetches from your DB. Options: JSON tool declaration in the SDK, an MCP server, or both. When would you pick which?

## Exercises

- **01-basic:** Make your first SDK call with prompt caching and tool use.
- **02-cumulative:** Build a small production-shape SDK app that ports *one* workflow from Claude Code. *Integrates any skill/persona/command from earlier sections that you port.*
