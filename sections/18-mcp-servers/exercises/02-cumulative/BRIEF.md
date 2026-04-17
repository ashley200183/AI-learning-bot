# Exercise 18.2 — Cumulative: Write a Minimal MCP Server

**Time estimate:** 3–4 hours
**Dependencies:** Python 3.10+ or Node 18+ (pick one); MCP SDK for that language
**Integrates:** S4 (tool-scoping discipline), S16 (debugging framework)

## Objective

Build a minimal MCP server that exposes two tools and one resource. Wire it into Claude Code via `.mcp.json`. Use it in a real task. Debug at least one thing that goes wrong.

## Tool choice

Pick one:
- **Python MCP SDK** — `pip install mcp`
- **TypeScript MCP SDK** — `npm install @modelcontextprotocol/sdk`

Document your choice in `submission/choice.md` with rationale.

## Requirements

### Part A — the server

Build at `submission/server/` a server that exposes:

- **Tool 1:** something useful in your domain. Examples:
  - Read-only: `list_recent_commits(n: int) -> list[commit]` — git log shortlog
  - Read+Compute: `estimate_bundle_size(entry_file: str) -> int` — parse imports, compute byte size
  - Transform: `render_template(template_name: str, vars: dict) -> str`
- **Tool 2:** something different enough from Tool 1 that you exercise the contract twice with different shapes.
- **Resource:** a readable data source. Examples:
  - `recent-errors` — your app's error log, last N lines
  - `schema-docs` — database schema from `schema.sql` or equivalent

The implementation can be trivial — the point is the MCP contract, not the logic.

### Part B — wire it

1. Add to your `.mcp.json`:
   ```json
   {
     "mcpServers": {
       "my-learning-server": {
         "command": "python",
         "args": ["submission/server/main.py"]
       }
     }
   }
   ```
   (Adjust for TS.)
2. Restart Claude Code. Verify tools and resource show up — ask Claude to list them.

### Part C — use it

1. Design a realistic task that requires at least one call to each tool and one read of the resource.
2. Execute. Capture in `submission/run.md`.

### Part D — deliberate break

Intentionally break something. Options:
- Remove a required field from the tool schema — Claude should report the tool as unusable
- Return invalid JSON from a tool call
- Leave out error handling and let an exception propagate

Observe Claude's behavior. Use your S16 framework to classify the failure (capability? instruction? trigger?). Capture in `submission/debug.md`:
- What you broke
- What Claude saw (error message)
- How you'd fix it
- Classification per S16

Restore the server to working.

### Part E — reflection

`submission/reflection.md`:
- Tool scoping (S4): what tools did you *not* expose, and why?
- When would you convert one of your existing skills into an MCP tool? When wouldn't you?
- What's the cost of the MCP layer vs a skill + direct script?

## Submission

- `submission/choice.md`
- `submission/server/` — full server code
- `submission/run.md`
- `submission/debug.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 26 / 35 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Server runs** — Claude sees tools + resource | `[GATE]` | /5 | Gate: doesn't connect = fail. This is the most common failure; budget time for debug. |
| **Two tools with different shapes** | `[GATE]` | /5 | Gate: two trivial identical tools = fail |
| **Resource readable** | — | /5 | |
| **Real task exercised all three** | `[GATE]` | /5 | Gate: one tool never used = unused exposed surface = fail |
| **Deliberate break + debug analysis** | `[GATE]` | /5 | Gate: if you didn't actually break it or didn't see the failure, fail. The point is to learn what MCP failures look like. |
| **Tool-scoping discipline** — explicit un-exposed tools | — | /5 | S4 integration |
| **Skill vs MCP argument** — honest | — | /5 | |

## What "good" looks like

Python server exposes `list_recent_commits(n)` and `file_line_count(path)`. Resource: `recent-test-failures` reading `.test-results.log`. Task: Claude uses the commit list to find recent changes, reads the failures resource, diffs to identify which commit likely broke the tests. Deliberate break: you remove the `n` parameter's type annotation. Claude reports *"I can't call this tool reliably — the schema says `n` is required but doesn't specify type. I'll guess integer."* You classify as *capability* (schema contract incomplete). Reflection: *"Convert a skill to MCP when I want another Claude client (Desktop, another Code session, remote tier-3 agent) to use it. Keep as skill if it's local-session only."*

## Integration with prior sections (targeted)

- **S4 — load-bearing.** Tool-scoping discipline. What not to expose is as important as what to.
- **S16 — load-bearing.** Debugging framework classifies the deliberate-break failure.
