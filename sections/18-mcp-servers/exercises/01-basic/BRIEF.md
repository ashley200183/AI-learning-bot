# Exercise 18.1 — Use an Existing MCP Server

**Time estimate:** 1.5–2 hours
**Dependencies:** Claude Code CLI; access to at least one MCP server

## Setup — pick a server you have access to

You likely already have at least one MCP server configured (this Claude Code environment lists several). Good candidates:

- **chrome-devtools** — web automation; tools like `take_screenshot`, `evaluate_script`, `list_network_requests`
- **playwright** — browser automation (similar territory)
- **stripe** — Stripe API tools if you have credentials
- **Neon** — Postgres hosting, good for DB work

If none are already configured, follow your MCP client's docs to add one (`.mcp.json` config is the standard path). Verify with: ask Claude to list tools from the server — you should see them.

## Objective

Use at least 3 different tools from the server you picked. Capture what MCP gave you that wouldn't have been possible with a skill + Bash.

## Requirements

1. In `submission/server-choice.md`: which server, why, what you have access to.
2. Design a real task that uses the server. Examples:
   - chrome-devtools: *"Open a URL, screenshot, grep console for errors, report status."*
   - Neon: *"Describe the schema of the project's main DB; find tables with no primary key."*
   - stripe: *"List customers created in the last 7 days; flag any without active subscriptions."*
3. Execute the task. Claude will invoke MCP tools. Capture in `submission/run.md`:
   - Which tools fired (minimum 3 distinct)
   - What each returned (key excerpts)
   - Total time to complete vs what you'd estimate for writing a bespoke script
4. In `submission/what-mcp-gave-you.md`: for each of the 3 tools, answer — "what would I have had to write to replicate this as a skill + Bash command, and what's lost?"
5. Write `submission/reflection.md`:
   - What was the most useful tool? Why?
   - What broke / surprised you?
   - When is MCP overkill? (There is such a time.)

## Submission

- `submission/server-choice.md`
- `submission/run.md`
- `submission/what-mcp-gave-you.md`
- `submission/reflection.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Three distinct tools used** | `[GATE]` | /5 | Gate: same tool three times = fail |
| **Real task execution** — not documentation-only | `[GATE]` | /5 | Gate: synthetic or dry-run = fail |
| **Skill+Bash counterfactual** — specific per tool | — | /5 | |
| **MCP-overkill case** — honest | — | /5 | Not every integration deserves MCP |

## What "good" looks like

You use chrome-devtools: `new_page`, `evaluate_script`, `take_screenshot`. Run: open staging URL, run a JS expression to capture render-timing, screenshot for visual diff. Your skill+Bash counterfactual: "I'd need Puppeteer, a screenshot tool, and a shell script around them. ~100 lines, plus dependency management. MCP delivered this in zero custom code." Overkill case: *"If I needed to screenshot one URL once a week, MCP is overkill — a one-liner with `curl` + a screenshot API is fine."*

## Integration with prior sections

None — introduces MCP mechanics.
