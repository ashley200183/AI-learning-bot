# Section 18 — MCP Servers

**Source:** NEW — Model Context Protocol, not covered in the original plan
**Prerequisites:** Sections 1–17 complete
**Estimated time:** 3–4 hours

## What you'll walk away knowing

- What MCP (Model Context Protocol) is and why it exists — a standard way to expose tools/data/resources to Claude
- How to configure an existing MCP server in Claude Code (`.mcp.json`)
- The three MCP resource types: tools (function calls), resources (readable data), prompts (reusable templates)
- When MCP is the right answer vs a skill with a shell command vs a custom subagent
- How to write a minimal MCP server in TypeScript or Python

## Why this section exists

Skills encode *how*. MCP servers encode *what Claude can reach*. For any AI system that integrates with external systems — databases, APIs, internal tools, private docs — MCP is the durable, shareable integration layer. If you want "fully customised AI systems" that talk to your actual infrastructure, MCP is the mechanism.

## Concepts the teacher will work through with you

1. **Protocol not framework.** MCP defines a JSON-RPC contract between Claude and a server. Any language can implement either side.
2. **Three resource types:**
   - **Tools** — functions Claude can call (with typed args; schema exposed via the protocol)
   - **Resources** — data Claude can read (database rows, files, API responses)
   - **Prompts** — server-side prompt templates Claude can instantiate
3. **Transport.** stdio (local, most common), SSE/HTTP (remote). Claude Code connects via `.mcp.json`.
4. **When MCP vs skill+Bash.** Skill + Bash: quick, one-shot, local. MCP: typed contract, reusable, shareable, works across Claude clients.
5. **When MCP vs subagent.** Subagent: reasoning + action on your behalf. MCP: raw capability Claude can compose into its own reasoning.

## Check-for-understanding questions

- A shell script that greps a log file vs an MCP server that exposes the same — when does each win?
- Why is the JSON-RPC schema important? What breaks if it's missing?
- You want Claude to query Linear for open issues. MCP tool, MCP resource, or both?
- Which is more portable between Claude clients (Code, Desktop, API): a skill or an MCP server?

## Exercises

- **01-basic:** Configure an existing MCP server (one you already have access to) and use at least three of its tools in real work.
- **02-cumulative:** Write your own minimal MCP server. *Integrates S4 (tool-scoping discipline), S16 (debugging framework when the server breaks).*
