# Agentic Flows: Learning Plan (v3)

> Applied learning — every concept starts simple, gets technical, and always includes a concrete example.
> Every phase has a sandbox exercise and a project application section.
> Do the project application before moving to the next phase.

---

## Phase 1 — Mental Models: Skills, Personas, Agents

### What is a skill?

**Simple version:** A skill is a cheat sheet Claude reads before doing a specific job. You write it once, it lives in a file called `SKILL.md`, and whenever that kind of task comes up Claude pulls it out automatically.

**Technical version:** A skill is a folder with a `SKILL.md` file containing YAML frontmatter (`name:`, `description:`) plus optional scripts and reference files. Claude's runtime scans all installed skills' frontmatter at session start (~100 tokens each). When your message matches a description, the full skill body loads into context. This is progressive disclosure: expensive content only costs tokens when needed.

**Example — a commit message skill:**
```yaml
---
name: commit-style
description: >
  Use whenever writing a git commit message, running git commit,
  or preparing to commit code. Use even for small or "obvious" commits.
---
# Commit message format
Always use Conventional Commits:
  feat(scope): short description
  fix(scope): short description
## Rules
- Subject line: 50 chars max, imperative mood ("add" not "adds")
- Body: explain *why*, not *what*
```
Now every commit in your project follows the same format — zero reminders needed.

---

### What is a persona?

**Simple version:** A persona is telling Claude "for this task, think like a specific type of expert." It changes *how* Claude reasons, not what tools it has.

**Technical version:** A persona is a markdown file that prepends a role definition, priority framework, and decision-making style to Claude's context. It shifts what Claude *attends to* in the input — a security persona notices SQL injection in code that a feature-delivery persona would approve.

**Example — security persona vs default:**

Code: `query = f"SELECT * FROM users WHERE id = {user_id}"`

Without persona → "Looks good, retrieves the user by ID."

With security persona → "SQL injection vulnerability — `user_id` is interpolated directly. Use `db.execute('SELECT * FROM users WHERE id = ?', [user_id])`. Critical issue."

Same code. Completely different output.

---

### What is an agent?

**Simple version:** An agent is Claude with hands — it reads files, runs code, makes decisions, and takes actions without you typing every step.

**Technical version:** A subagent is a Claude instance spawned by another Claude instance to handle a scoped task. It has its own context window, its own tool permissions, and reports back when done. The parent only sees the final output — the subagent's internal reasoning stays hidden.

**Example — a test-writing subagent:**
```
Task: Write tests for /src/payments/processor.ts
Tools: Read, Write, Bash (npm test only)
Done when: All tests pass, coverage >80%
Return: Summary of what was tested and what was skipped
```
Your main session continues planning the UI while the subagent handles tests. Your context stays clean.

---

### How they relate

| | Skill | Persona | Agent |
|---|---|---|---|
| Changes | Procedure ("how to do X") | Judgment ("how to think") | Capability ("go do X") |
| Loaded when | Task matches description | Called explicitly | Explicitly spawned |
| Analogy | A recipe | A job title | A coworker given a task |

---

### Sandbox exercise
1. Install Claude Code: `/plugin marketplace add anthropics/skills` then `/plugin install example-skills@anthropic-agent-skills`
2. Ask: "Create a PDF summary of this project" — watch whether it names the PDF skill
3. If it doesn't, say "Use the PDF skill to create a PDF summary" — note the difference
4. That gap is what Phase 2 and Phase 4 close

### Apply to your project
1. Audit your last 10 conversations — write down every instruction you typed more than once
2. List 2–3 reviewer personas (what does each care about that a general Claude doesn't?)
3. Find 3 tasks Claude could do in isolation with a clear success criterion
4. Store everything in `notes/agent-plan.md` — you'll fill it in each phase

---

## Phase 2 — Build Your First Skill

### Anatomy of a SKILL.md

```
my-skill/
├── SKILL.md          ← required: frontmatter + main instructions
├── scripts/          ← optional: Python/bash Claude can run
└── references/       ← optional: deep docs loaded on demand
```

**The description field is everything.** It's the only content scanned at session start. Make it pushy — tell Claude when to use the skill, not just what it does.

**Example — weak vs strong description:**
```yaml
# WEAK — skipped for most tasks
description: "How to write API endpoints"

# STRONG — triggers reliably
description: >
  How to design and write REST API endpoints for this project.
  Use whenever adding a new route, modifying an endpoint, writing
  controller logic, or when the user mentions API, endpoint, route,
  or handler — even if the request seems simple.
```

**Example — progressive disclosure in a skill:**
```
api-design/
├── SKILL.md           ← loads on trigger: routing conventions, response format
└── references/
    ├── auth-patterns.md     ← loads only if auth is mentioned
    └── error-codes.md       ← loads only if error handling is needed
```
For a simple GET endpoint, the reference files never load. For an auth-protected route, `auth-patterns.md` loads automatically. Tokens only when needed.

---

### Sandbox exercise
1. Install skill-creator: `/plugin install skill-creator@anthropic-agent-skills`
2. Say: "Use the skill-creator to help me build a skill for writing commit messages"
3. Answer its questions — it generates a complete SKILL.md and tests trigger reliability
4. Watch what it changes in the description — that's the lesson

### Apply to your project
1. Pick the highest-value repeating task from your Phase 1 audit
2. Use skill-creator to build it (don't handwrite the first draft)
3. Write 5 test prompts you'd actually type — run them, track which fire
4. Iterate the description until 4/5 trigger reliably
5. Install at project scope: `.claude/skills/your-skill/`

**Repos:** `anthropics/skills` (skill-creator) · `dscv103/claude-skills-cli` · `daymade/claude-code-skills`

---

## Phase 2.5 — Commands

### Skills vs commands — the unified system

**Simple version:** Commands are things *you* type to trigger explicitly, like a keyboard shortcut (`/deploy`, `/review`). Skills are things *Claude* triggers automatically based on context. Both look the same from the outside (a `/name`) but one waits for you to call it, the other calls itself.

**Technical version:** In 2026, custom commands and skills merged into one system. A file at `.claude/commands/deploy.md` and a skill at `.claude/skills/deploy/SKILL.md` both create the same `/deploy` command and work identically. The distinction now lives in *intent*: commands are for user-invoked workflows, skills are for agent-invoked capabilities. The frontmatter `disable-model-invocation: true` makes a skill command-only — Claude won't auto-trigger it.

**Example — same capability, two triggers:**
```
# As a skill (auto-triggers when relevant)
description: "Format and validate API responses. Use whenever the user
receives or writes API response code, even if they don't ask for formatting."

# As a command (you invoke it deliberately)
description: "Format and validate API responses."
disable-model-invocation: true   ← Claude never auto-loads this
```
Use the command version for things you want explicit control over. Use the skill version for things that should always apply.

---

### Built-in commands worth knowing

| Command | What it does | When to use |
|---|---|---|
| `/compact` | Summarises conversation, frees context | Proactively before starting a new task phase |
| `/compact Focus on the API changes` | Directed compaction | When you want to keep specific context alive |
| `/clear` | Wipes the context window entirely | Between completely unrelated tasks |
| `/diff` | Shows all changes Claude made this session | Checkpoint before moving on |
| `/btw` | Asks Claude a question without adding it to history | Quick factual checks mid-task |
| `/review` | Activates code review persona | After writing a chunk of code |
| `/simplify` | Reduces over-engineering | When code is getting complex |
| `/batch` | Groups related tasks to run together | Multiple similar small tasks |
| `/loop` | Runs a recurring task on a cycle | Continuous test-fix loops |
| `/model` | Switches models mid-session | Routing expensive tasks to Opus, cheap ones to Haiku |
| `/cost` | Shows current session token usage | Cost awareness during long sessions |

**The most underused:** `/btw`. It fires a question in an overlay that never enters conversation history. Perfect for checking a detail without inflating your context window.

---

### Custom command frontmatter options

```yaml
---
name: deploy
description: Deploy to staging environment
allowed-tools: Bash(npm run deploy:*)    # Restrict which tools this command can use
argument-hint: <environment>              # Shows in autocomplete: /deploy staging
disable-model-invocation: true           # Never auto-trigger — command only
context: fork                            # Run in a forked agent (see Phase 5.5)
agent: Explore                           # Which agent type to use when forked
---
```

---

### Apply to your project

1. **Run `/diff` at the end of your next Claude session.** Review everything it changed. Add a rule to CLAUDE.md for anything it changed that surprised you.
2. **Build one custom command** for a task you invoke deliberately. Good candidates: `/deploy`, `/release-notes`, `/update-changelog`, `/pr-description`.
3. **Use `/btw` for 5 questions** in your next long session. Notice how the context stays leaner.
4. **Use `/compact Focus on [current task]`** before switching phases in a long session. Then use `/diff` after — see what survived.

---

## Phase 3 — Personas & Agents

### Personas in depth

**Simple version:** You're giving Claude a job title and telling it what that person cares about. The QA engineer looks for ways to break things. The security engineer looks for vulnerabilities. The same code review request produces completely different feedback depending on who you ask.

**Technical version:** A persona is a system-prompt prefix establishing cognitive framing before any task runs. It sets priorities, risk tolerance, and what patterns to look for. gstack's slash commands are the most widely-deployed example:

```bash
/plan-eng-review    # "What are the technical risks here?"
/qa                 # "How do I break this?"
/review             # "I'm seeing this code for the first time"
/plan-ceo-review    # "Is this the right thing to build?"
```

**Example — the same feature spec through different personas:**
- `/plan-eng-review`: "The state machine in step 3 has a race condition under high load"
- `/qa`: "What happens if the user submits twice before the first request returns?"
- `/plan-ceo-review`: "This solves a problem 3% of users have — is this the right sprint priority?"

None of those come from default Claude. Each persona literally changes what Claude looks for.

---

### Agents as role + capability

**Simple version:** An agent is a persona that can act autonomously, not just advise. It's not "think like a QA tester" — it's "be a QA tester, run the tests, file the bugs, done."

**Technical version:** A `.claude/agents/your-agent.md` file combines a system prompt with a tool allowlist and model specification. When spawned, it works in its own context window. The parent defines what files it can touch and what "done" means.

**Example — a domain-specific agent:**
```yaml
---
name: security-reviewer
description: Reviews code for security vulnerabilities
tools: Read, Grep, Glob, Bash
model: opus
---
You are a senior security engineer. Review code for:
- SQL/command injection
- Auth and authorization flaws
- Secrets or credentials in code
- Insecure data handling
Provide specific line references and suggested fixes.
```

Invoke with: "Use a subagent to review this code for security issues."

---

### Sandbox exercise
1. Install gstack: `git clone https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && ./setup`
2. Open your project, write or find 20 lines of code
3. Ask for a review without any persona — record feedback
4. Run `/review` on the same code — compare
5. Run `/qa` on the same code — compare again
6. Write down: what did each catch that the others missed?

### Apply to your project
1. Browse `alirezarezvani/claude-skills` for a persona close to what you need
2. Install it and run it on real code from your project
3. Build a custom persona specific to your codebase using skill-creator
4. Install as `.claude/agents/project-reviewer.md`
5. Test: does the custom persona give more useful feedback than a generic one?

**Repos:** `garrytan/gstack` · `alirezarezvani/claude-skills`

---

## Phase 3.5 — Context Windows, Memory & Persistence

### What is a context window?

**Simple version:** Claude has a notepad. Everything in the current conversation goes on that notepad. When the notepad gets full, older notes get pushed off. Once something is off the notepad, Claude has forgotten it — unless it's written somewhere Claude can re-read it.

**Technical version:** The context window is the fixed-length token buffer Claude processes with each request. Claude Sonnet 4.6 has a 200K token window. Every message, file read, tool result, and system prompt occupies tokens. As the window fills: performance degrades, Claude starts contradicting earlier decisions, and eventually auto-compaction triggers — which summarises the conversation and discards the details.

**Example — context degradation in practice:**
Session starts at 8am. You're building an auth system. Claude is sharp, consistent, remembers every decision.

By midday — 180K tokens in — Claude suggests using `session_id` as a security token. It was told explicitly three hours ago that this is insecure. The instruction is still in the window, but it's been crowded out by 40 file reads and a hundred tool calls. Claude's performance has degraded.

The solution isn't a bigger window. It's better management of the existing window.

---

### The three context management strategies

#### 1. Compaction — distilling what matters

**Simple version:** When the notepad gets too full, compress the old pages into a summary. You lose detail but keep the important decisions.

**Technical version:** Claude Code's auto-compaction triggers at ~80% context capacity and runs a structured LLM summarisation pass over the conversation, producing a 9-section summary covering: current state, key decisions, modified files, patterns established, errors resolved, and remaining work. This summary replaces the raw conversation history.

You can also trigger it manually with `/compact` and guide what survives:
```
/compact Focus on: the API schema decisions, the auth token approach, and the list of modified files
```

**Customise what survives in CLAUDE.md:**
```markdown
## Compaction instructions
When compacting, always preserve:
- Full list of modified files
- All architecture decisions and their rationale
- Active test commands
- Any patterns explicitly established in this session
```

---

#### 2. Tool-result clearing — dropping the noise

**Simple version:** After you've read a file and used what you needed, the full file content sitting in your context is dead weight. Drop it.

**Technical version:** As an agent calls tools (file reads, bash commands, web fetches), tool results pile up in the context. Most of them are re-fetchable — if Claude needs to see `auth.ts` again, it can just re-read it. Tool-result clearing drops old, bulky tool outputs while keeping the record that the call happened. This is the main source of token bloat in long coding sessions.

**Practical impact:** In a 15K-token session, tool results typically account for 80%+ of the tokens. Clearing old ones is 5–10x more effective than compacting the prose conversation.

---

#### 3. Memory — surviving across sessions

**Simple version:** When the session ends, the notepad is thrown away. Memory is a separate filing cabinet Claude writes to and reads from — so the next session can pick up where the last one left off.

**Technical version:** There are three memory locations:
- `CLAUDE.md` — always-loaded standing instructions (project context, conventions)
- `notes/` files — markdown files Claude writes to and references explicitly
- The memory tool (API) — structured key-value store Claude drives itself

The most practical approach for Claude Code: have Claude write a `notes/session-handoff.md` at the end of each session:

```markdown
## Session handoff — [date]
### What was done
- Implemented JWT refresh token rotation in /src/auth/tokens.ts
- Added Zod validation to all /api/auth routes

### Decisions made
- Using httpOnly cookies for token storage (not localStorage) — security concern
- Refresh tokens expire in 7 days, access tokens in 15 minutes

### What's next
- Add rate limiting to /api/auth/login (not done)
- Write integration tests for token rotation

### Files modified
- src/auth/tokens.ts
- src/api/auth/login.ts, refresh.ts
- src/middleware/requireAuth.ts
```

Start the next session with: "Read notes/session-handoff.md and continue from where we left off."

---

### Context in multi-agent vs solo sessions

**Solo agent:** One context window, everything accumulates. Long sessions degrade. Best managed with frequent compaction and subagent delegation for research.

**Parallel agents (Agent Teams / worktrees):** Each agent has its own clean context window. The auth agent only knows about auth. The test agent only knows about tests. Neither gets polluted by the other's work. This is why parallel agents often *outperform* a single long session — not just because they're faster, but because each agent starts and stays fresh.

**When to use which:**

| Scenario | Best approach | Why |
|---|---|---|
| Single focused task (<2 hours) | Solo session | Overhead not worth it |
| Multi-file feature with independent concerns | Parallel agents + worktrees | Each agent stays focused |
| Long exploratory session | Solo with frequent /compact | Preserve continuity |
| Known repeatable task (tests, migrations) | Subagent | Isolate context, clean output |
| Cross-session work | Memory files + handoff docs | Survive context wipe |

---

### Agent-to-agent handoff tactics

When one agent finishes and another begins, context transfer is the hard part. These patterns work:

**1. Structured handoff document** (most reliable):
```
Phase 1 complete. 
Decisions made: [list]
Files created/modified: [list with paths]
What worked: [list]
What was explicitly rejected: [list — important for avoiding re-litigating]
Open questions for Phase 2: [list]

Next: Load [persona] and [skills], read this document first.
```

**2. Shared state file** (for long multi-phase work):
Create `notes/project-state.md` and have each agent update it on completion. New agents read it first.

**3. CLAUDE.md inheritance** (automatic):
All agents in Claude Code — subagents, teammates, worktree sessions — load your project's CLAUDE.md. Your conventions and persona routing travel with every agent automatically.

**4. Git history as memory** (often overlooked):
Agents can read `git log --oneline` and `git diff HEAD~5` to understand what changed recently. This is often more accurate than a handoff document for understanding *what was actually done*.

---

### Sandbox exercise

1. Run a 30-minute Claude Code session on your project without managing context
2. Watch `/cost` — note the token count at different points
3. Start a fresh session, but this time: use `/btw` for quick questions, `/compact` before phase changes, and use a subagent for one file-heavy research task
4. Compare final token count and Claude's consistency in the two sessions

### Apply to your project

1. **Add compaction instructions to your CLAUDE.md** (what must survive any compaction)
2. **Create `notes/session-handoff.md`** — ask Claude to update it at the end of each session. Add this to your CLAUDE.md: "At the end of every session, update notes/session-handoff.md with what was done, what was decided, and what's next."
3. **Add a PreCompact hook** that saves a snapshot before compaction fires:
   ```json
   {
     "hooks": {
       "PreCompact": [{
         "hooks": [{
           "type": "command",
           "command": "cp notes/session-handoff.md notes/pre-compact-snapshot-$(date +%Y%m%d-%H%M).md"
         }]
       }]
     }
   }
   ```
4. **Test memory across sessions:** End a session with Claude updating the handoff doc. Start a new session, say "Read notes/session-handoff.md and continue." Compare continuity to starting cold.

---

## Phase 4 — Wire It Together: CLAUDE.md & Forced-Eval

### CLAUDE.md as standing orders

**Simple version:** `CLAUDE.md` is a sticky note on Claude's monitor. It's there every session, telling Claude who you are, what project this is, and what rules to always follow — including "check your cheat sheets before doing anything."

**Technical version:** `CLAUDE.md` is injected into every Claude Code session automatically. It's always present — unlike skills (which load on demand). This makes it the right place for: project context, conventions, persona routing rules, and the forced-eval instruction.

**Example — a complete, effective CLAUDE.md:**
```markdown
# My Project
A SaaS invoicing platform. Stack: Next.js 14, PostgreSQL, Prisma, tRPC.
Beta with 12 customers. Stability > speed.

## Skill activation — mandatory
Before any task:
1. List skills that could apply (even 1% relevance counts)
2. State which you're invoking and why, or why none apply
3. Then proceed

Never skip this step. Write it out every time.

## Always apply
- All DB queries use Prisma — never raw SQL
- All API routes validate input with Zod
- Never log: email, stripeCustomerId, taxId

## Persona routing
- Code review → agents/project-reviewer.md
- Security → agents/security-auditor.md

## Compaction: preserve
- Full list of modified files
- All architecture decisions and rationale
- Active test commands

## What not to do
- Don't suggest class-based React components
- Don't propose dependencies Prisma/tRPC already handles
```

---

### The forced-eval hook pattern

**Simple version:** Make Claude write "I checked my cheat sheets: [list], I'm using [X] because [Y]" before doing anything. Once it's written, it can't be skipped.

**Technical version:** The undertriggering problem exists because Claude can silently skip skills. The forced-eval pattern requires Claude to externalise its skill evaluation. Testing shows this pushes trigger reliability from ~40% (passive CLAUDE.md suggestion) to ~84% (forced written evaluation).

**Example — forced-eval in a real session:**

You ask: "Add a new endpoint for getting invoice totals by customer."

Without forced-eval → Claude starts writing code immediately.

With forced-eval → Claude writes:
> *Checking skills: api-design applies (new endpoint), commit-style will apply at end. Using api-design skill.*
> *Here's the endpoint...*

That single line means your conventions are applied and the commit format isn't forgotten.

---

### Sandbox exercise
1. Create a CLAUDE.md with the forced-eval block
2. Ask 5 different tasks — count how many times Claude writes a skill evaluation
3. Remove the block and repeat — compare trigger rates

### Apply to your project
1. Create your CLAUDE.md using the example template above
2. Add forced-eval block, project context, conventions, persona routing
3. Add compaction preservation rules
4. Run one full feature request through it — does it check skills? Apply conventions?
5. Add a rule every time Claude does something you have to correct

**Repos:** `obra/superpowers` · `daymade/claude-code-skills`

---

## Phase 4.5 — Hooks

### What is a hook?

**Simple version:** A hook is an alarm that fires at a specific moment in Claude's lifecycle and runs your script. "Before Claude runs any bash command, run my security check." Hooks are deterministic — unlike skills (Claude may ignore), hooks always run.

**Technical version:** A hook is a shell command in `.claude/settings.json` that fires at a lifecycle event. Your script receives a JSON payload, inspects it, and controls what happens via exit code: 0 = proceed, 2 = block (your stderr message goes to Claude), other non-zero = warn. `SessionStart` and `UserPromptSubmit` stdout gets added to Claude's context. All other events log to debug.

**Example — 6 production hooks:**

**1. Inject git context at session start:**
```json
{
  "hooks": {
    "SessionStart": [{
      "hooks": [{
        "type": "command",
        "command": "echo '{\"additionalContext\": \"Branch: '$(git branch --show-current)'. Uncommitted: '$(git status --short | wc -l)' files.\"}'"
      }]
    }]
  }
}
```

**2. Inject sprint context with every prompt:**
```json
{
  "hooks": {
    "UserPromptSubmit": [{
      "hooks": [{
        "type": "command",
        "command": "cat $CLAUDE_PROJECT_DIR/notes/current-sprint.md"
      }]
    }]
  }
}
```

**3. Block dangerous bash commands:**
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "echo \"$CLAUDE_TOOL_INPUT\" | grep -qE 'rm -rf|DROP TABLE|push --force' && echo 'Blocked: destructive command' && exit 2 || exit 0"
      }]
    }]
  }
}
```

**4. Auto-format TypeScript after every edit:**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{
        "type": "command",
        "command": "if [[ \"$CLAUDE_FILE_PATHS\" =~ \\.(ts|tsx)$ ]]; then prettier --write \"$CLAUDE_FILE_PATHS\"; fi"
      }]
    }]
  }
}
```

**5. Run tests after Claude finishes (Stop hook):**
```json
{
  "hooks": {
    "Stop": [{
      "hooks": [{
        "type": "command",
        "command": "INPUT=$(cat); [ \"$(echo $INPUT | jq -r '.stop_hook_active')\" = 'true' ] && exit 0; npm test 2>&1 | tail -5 || exit 2"
      }]
    }]
  }
}
```

**6. Desktop notification when Claude needs input:**
```json
{
  "hooks": {
    "Notification": [{
      "hooks": [{
        "type": "command",
        "command": "osascript -e 'display notification \"Claude needs your input\" with title \"Claude Code\"'"
      }]
    }]
  }
}
```

---

### CLAUDE.md vs skill vs hook — which to use

| | CLAUDE.md | Skill | Hook |
|---|---|---|---|
| Reliability | Claude may ignore rules | Depends on description | Guaranteed — always fires |
| Trigger | Passive | Semi-active (matching) | Deterministic (your code runs) |
| Best for | Conventions, standing context | Procedural knowledge | Automation, blocking, validation |
| Example | "Always use Zod" | "Here's how to build an endpoint" | "Block any edit to .env files" |

---

### Sandbox exercise
1. Add this minimal hook to `.claude/settings.json`:
   ```json
   {
     "hooks": {
       "UserPromptSubmit": [{
         "hooks": [{
           "type": "command",
           "command": "echo 'Check installed skills before responding. List which apply, state your choice, then proceed.'"
         }]
       }]
     }
   }
   ```
2. Submit any prompt — Claude opens with a skill check
3. This is the hook version of forced-eval. The difference: it can't be forgotten

### Apply to your project
1. Start with the most annoying problem: sprint context injection, auto-format, or dangerous command blocking
2. Add it to `.claude/settings.json` and test on a real session
3. Create `notes/current-sprint.md` with: what you're building, what's blocked, what not to touch
4. Add the test-on-Stop hook — your code is tested every time Claude finishes a response

**Repos:** `disler/claude-code-hooks-mastery` · Official docs: `code.claude.com/docs/en/hooks`

---

## Phase 5 — Multi-Agent Thinking

### The mental model shift

**Simple version:** Single agent = pair-programming. Multi-agent = managing a team. You define the goal, specialists work in parallel, you review the combined output.

**Technical version:** With multiple agents, each has its own clean context window. No single agent's context gets polluted by the full project history. Each worker starts fresh with a scoped brief. The gain: parallelism, specialisation, and work beyond any single context window's capacity.

**Example — single vs multi-agent for authentication:**

Solo: You guide step by step. Claude writes middleware, you review. It writes the login page, you review. Sequential. Your context fills with auth implementation details.

Multi-agent team:
- Agent A (architect): designs token schema and flow, writes a decision doc
- Agent B (implementer): builds from the doc
- Agent C (tester): writes integration tests as B builds

Three agents run simultaneously. You review combined output. Your main context stays clean.

---

### Subagents (start here)

**Simple version:** You stay in one conversation but can hand off self-contained jobs to a helper that works in isolation.

**Example — subagent for migration generation:**
```
"Spawn a subagent to:
1. Read /prisma/schema.prisma
2. Read /notes/payments-schema-requirements.md
3. Generate the migration file
4. Verify with 'npx prisma validate'
5. Return: migration contents and any warnings"
```
You continue planning while the subagent handles the database work.

---

### Agent Teams (true parallelism)

Enable: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=true`

**Example:**
```
"Create an agent team to review PR #142.
Spawn three reviewers:
- security-reviewer: auth, input validation, secret exposure
- performance-reviewer: N+1 queries, re-renders, bundle size
- test-reviewer: coverage gaps, edge cases, flaky tests
Have them review and report findings."
```
Three simultaneous reviews. Lead synthesises. You get coverage no single reviewer could provide.

---

## Phase 5.5 — Forking Conversations

### What is a forked conversation?

**Simple version:** You're having an exploratory planning conversation with Claude. You want to hand off a concrete task to a worker agent without contaminating your planning context with all the implementation details. Forking spins off an execution agent while your main conversation stays clean for big-picture thinking.

**Technical version:** The `context: fork` frontmatter directive in a skill or command causes Claude to run that skill in a separate agent context rather than the main conversation. The fork agent executes the task, then summarises results back to the main conversation. The main context only receives the summary — not the full execution trace, file reads, or tool calls. This is the native Claude Code mechanism for keeping your exploratory context clean.

**Example — a research fork skill:**
```yaml
---
name: deep-research
description: Research a topic thoroughly without polluting main context
context: fork
agent: Explore
---
Research $ARGUMENTS thoroughly:
1. Find relevant files using Glob and Grep
2. Read and analyze the code
3. Summarize findings with specific file references

Results are summarized and returned to your main conversation.
```

You ask: "Use deep-research to understand how our auth token refresh works."

The Explore agent reads all the relevant files — 20K tokens of file contents. Your main conversation receives a 500-token summary. Your planning context is barely touched.

---

### When to fork vs subagent vs just ask Claude

| Situation | Use | Why |
|---|---|---|
| Research a codebase area | Fork (Explore agent) | Read-only, large files, keep main clean |
| Execute a bounded task with output | Subagent | Needs tools, produces files/commits |
| Parallel independent tasks | Agent Teams + worktrees | True parallelism |
| Quick question, no context impact | `/btw` | Doesn't enter history at all |
| Complex task, need to guide closely | Main session | You need to redirect in real-time |

---

### The exploratory main / execution branch pattern

This is the most powerful use of forking:

1. **Main session (exploratory):** You design, make decisions, ask clarifying questions, iterate on the approach. Context stays focused on the high-level.
2. **Forked agents (execution):** Each concrete task — "implement this", "write tests for that", "migrate this schema" — runs in its own fork.
3. **You review outputs** from forks and bring only the decision ("keep this, discard that") back into the main conversation.

Your main session can run for hours and stay coherent because it never gets polluted with implementation noise.

---

### Apply to your project

1. **Create a deep-research skill** using the example above. Use it on one module you want to understand better before modifying.
2. **For your next multi-file task**, identify which parts are exploratory (you need to think and decide) vs executable (Claude can go do it). Use the main session for the former, forks for the latter.
3. **Compare the context cost:** run `/cost` after a session where you used forks vs one where you did everything inline. The difference is your "fork savings."

---

## Phase 5.75 — Git Worktrees

### What is a git worktree?

**Simple version:** Normally, your project exists in one folder. If Claude is working on a feature in that folder and you need to fix a bug, you have to interrupt it. Git worktrees let you have multiple copies of your project's files checked out simultaneously — each on a different branch — so multiple Claude sessions can work in parallel without stepping on each other.

**Technical version:** `git worktree add` creates a new working directory linked to the same repository. Each worktree has its own branch, its own files on disk, and its own modified state. They share commit history but not working directory state. Claude Code v2.1.49 added native first-class worktree support via the `--worktree` flag. Each session in a worktree gets its own isolated filesystem, its own context window, and its own Claude instance.

**Example — the before and after:**

Before worktrees:
```
You're 2 hours into building feature/payments.
Critical bug report arrives.
Options: stash everything (lose context), commit WIP (dirty history), or ignore the bug.
```

After worktrees:
```bash
# Terminal 1: keep feature session running
claude --worktree feature-payments   # Session A, unchanged

# Terminal 2: start bug fix in isolation
claude --worktree bugfix-auth-crash  # Session B, new branch, full codebase

# Both run simultaneously, zero interference
```

---

### How to use worktrees

**Basic usage:**
```bash
# Named worktree (recommended — names the branch too)
claude --worktree feature-payments

# Auto-named worktree
claude --worktree

# Creates: .claude/worktrees/feature-payments/ on branch worktree-feature-payments
```

**In subagent frontmatter (for automatic isolation):**
```yaml
---
name: refactor-agent
description: Performs isolated refactoring
isolation: worktree
---
```
Or just tell Claude: "Use worktrees for your agents."

**Manual git worktree (for specific branches):**
```bash
git worktree add ../project-feature-a -b feature-a
cd ../project-feature-a && claude
```

---

### Parallel agent patterns with worktrees

**Pattern 1 — Batch migration:**
You need to update 50 files from one API pattern to another.
- Spawn 5 agents, each in their own worktree, each handling 10 files
- All run simultaneously, no file conflicts
- Review and merge each branch when done

The built-in `/batch` command does this automatically.

**Pattern 2 — Competitive implementation:**
You're not sure which approach to take for a complex refactor.
- Agent A refactors `src/auth.ts` approach 1 (worktree-a)
- Agent B refactors `src/auth.ts` approach 2 (worktree-b)
- You review both branches and pick the winner (or cherry-pick the best parts)

This is genuinely impossible without worktrees — both agents would conflict on the same file.

**Pattern 3 — Feature + hotfix in parallel:**
```bash
claude --worktree feature-subscriptions   # Long-running feature
claude --worktree hotfix-payment-bug      # Urgent bug fix
```
Both run. You switch between terminal windows to check progress. When the hotfix is done, it merges to main independently of the in-progress feature.

---

### Worktree limits and gotchas

**Practical limit:** 3–10 parallel sessions before managing them becomes bottleneck. Beyond that, use Gas Town or Agent Teams.

**Shared resources are not isolated:** Git worktrees isolate the filesystem, not the database, environment variables, or running services. Two agents running migrations against the same local database will conflict. Use:
- Separate database schemas per worktree
- Docker containers with `.env` overrides
- Or keep DB-touching agents sequential

**Every worktree needs its own setup:**
```bash
# After claude --worktree feature-payments, inside that worktree:
npm install      # dependencies may be missing
cp .env.example .env  # env vars need to be set
```

**Cleanup is automatic when no changes were made.** If the agent made commits, Claude asks whether to keep or remove the worktree when you exit.

---

### Apply to your project

1. **Run two parallel worktrees** for the first time. Pick two independent tasks from your backlog. Assign one to each:
   ```bash
   claude --worktree task-one
   # New terminal:
   claude --worktree task-two
   ```
2. **Watch both sessions work.** Check progress by switching terminals. Experience the difference between sequential and parallel.
3. **Add `isolation: worktree`** to your custom agents for any agent that modifies files. It becomes the default for parallel safety.
4. **Build a batch migration template:** Pick a type of repetitive change in your project (updating import paths, adding type annotations, adding missing tests). Write it as a worktree-isolated subagent that takes a list of files and works through them.

---

## Phase 6 — Full Orchestration

### The three-tier model

- **Tier 1 (interactive):** Single session. Claude Code CLI. Best for single features, pair-programming.
- **Tier 2 (local parallel):** Multiple instances in isolated worktrees, supervisor agent. Tools: Gas Town, oh-my-claudecode. Best for sprint-scale work.
- **Tier 3 (cloud async):** Agents in cloud VMs. You assign, walk away. Tools: Claude Code Web. Best for overnight backlog draining.

**Example — Tier 2 in practice:**
Friday afternoon, 6 bugs to fix. Without orchestration: work through them one by one, finish 2.

With Gas Town:
1. Tell the Mayor: "Fix issues #234–239"
2. Mayor decomposes into beads
3. Six polecats spin up in isolated worktrees, each fixing a bug
4. They commit, open PRs
5. You come back to 6 PRs to review

Cost: ~$60. Time saved: 4–5 hours. Not worth it for a single bug — very worth it for a sprint.

---

### oh-my-claudecode — gentler Tier 2 entry

**Example — using `plan`:**

Normal session: "Add a payment webhook handler" → Claude immediately starts writing code.

With `plan`: Claude runs a planning interview first:
> "What payment provider? What events? What's the existing webhook infrastructure? What's the rollback plan?"

Three scope issues caught before a line is written.

---

### Apply to your project

1. Identify a first Tier 2 candidate: well-scoped, multi-file, lower risk
2. Prepare: solid CLAUDE.md, project-scope skills, clean git history, `notes/agent-brief.md`
3. Run oh-my-claudecode `plan` mode — review the plan before proceeding
4. Let it run without interrupting — review the full output
5. Debrief: cost, quality, what to add to CLAUDE.md to prevent its mistakes

**Repos:** `steveyegge/gastown` · `oh-my-claudecode`

---

## Phase 7 — Evals & Benchmarking

### What is an eval?

**Simple version:** An eval is a test for your AI workflow. You give it a specific input, run your workflow, and then check whether the output meets your criteria. If you've built a skill for writing commit messages, an eval checks: does it consistently produce Conventional Commits format? Does it summarise the right level of detail? Is it under 50 characters?

**Technical version:** An evaluation is an automated test that measures an AI system's behaviour on a defined task with a defined grading criterion. For agentic workflows, evals are more complex than single-turn — they may involve multi-step execution, tool use, environment interaction, and grading against verifiable end states rather than text similarity.

**Example — a skill eval in practice:**

You built a skill that generates PR descriptions. To evaluate it:
- **Input:** 5 different diffs (small fix, large refactor, new feature, dependency update, breaking change)
- **Expected behaviour:** Each gets a description that includes: what changed, why, how to test it
- **Grader:** An LLM judge checking for those three components

Run this eval every time you modify the skill. If the score drops, you broke something.

---

### Grader types

#### 1. Deterministic (exact match / tests passing)

**Simple version:** Either the code compiles and tests pass, or it doesn't. No judgment required.

**Technical version:** Verifiable outputs — test suites, lint checks, type checks, schema validation. The gold standard for coding agents. SWE-bench uses this: the patch either fixes the failing tests without breaking others, or it doesn't.

**Example:**
```bash
# Eval runner for a test-writing skill
claude "Write tests for src/auth/tokens.ts"
npm test -- --coverage src/auth/tokens.test.ts
echo "Coverage: $(cat coverage/summary.json | jq .tokens.pct)%"
# Pass if coverage > 80% and all tests green
```

---

#### 2. LLM-as-judge (rubric grading)

**Simple version:** You give another Claude session a rubric and ask it to score your workflow's output. It's like asking a teacher to grade a homework assignment.

**Technical version:** An LLM grader receives: the original task, the agent's output, and a scoring rubric. It returns a score and reasoning. Key rules: don't let the same model grade its own output (self-preference bias), define concrete scoring criteria (not "was this good?"), and calibrate with human reviews periodically.

**Example rubric for a PR description skill:**
```yaml
Grade the following PR description on this rubric (1-5 each):

Criteria:
1. Completeness: Does it explain what changed? (1=missing, 5=thorough)
2. Clarity: Can a reviewer understand it without reading the diff? (1=unclear, 5=clear)
3. Testability: Does it explain how to test the change? (1=missing, 5=specific steps)
4. Brevity: Is it appropriately concise? (1=too long/short, 5=right length)

PR Description to grade:
[output]

Return JSON: {"completeness": N, "clarity": N, "testability": N, "brevity": N, "reasoning": "..."}
```

---

#### 3. Heuristic graders (fast pattern checks)

**Simple version:** Simple rules that don't need an LLM. "Does it contain 'TODO'? Is it under 100 lines? Does it import the right modules?"

**Technical version:** Regex checks, length constraints, required term presence, static analysis, linting. Very fast, zero cost, but limited to surface-level properties. Best used as a pre-filter before the expensive LLM judge.

**Example:**
```python
def heuristic_grade_commit_message(message):
    checks = {
        "has_type_prefix": bool(re.match(r'^(feat|fix|chore|docs|style|refactor|test)\(.+\):', message)),
        "under_50_chars": len(message.split('\n')[0]) <= 50,
        "imperative_mood": not message.lower().startswith(('added', 'fixed', 'changed', 'updated')),
    }
    return checks, sum(checks.values()) / len(checks)
```

---

### Building a benchmark: the eval loop

**Simple version:** Write test cases, run your workflow on them, grade the outputs, store the scores, compare across versions. When you change your skill, run the eval again — did the score go up or down?

**Technical version:** A benchmark is a suite of input/output pairs with grading logic that can run continuously. The structure:

```
eval-suite/
├── cases/
│   ├── case-001.json     ← {input: "...", expected_properties: [...]}
│   ├── case-002.json
│   └── ...
├── graders/
│   ├── deterministic.py   ← test running, linting
│   ├── llm_judge.py       ← rubric-based LLM grading
│   └── heuristics.py      ← fast pattern checks
├── run_eval.sh            ← runs all cases, all graders, aggregates
└── results/
    └── run-2026-04-16.json ← scores, reasoning, cost
```

**The continuous eval loop:**

```bash
#!/bin/bash
# run_eval.sh — run on every skill change

SKILL_VERSION=$(git rev-parse --short HEAD)
RESULTS_FILE="results/run-$SKILL_VERSION.json"

for case in cases/*.json; do
    INPUT=$(jq -r '.input' $case)
    
    # Run the workflow
    OUTPUT=$(claude -p "$INPUT" --no-stream)
    
    # Grade it
    DET_SCORE=$(python graders/deterministic.py "$OUTPUT")
    HEUR_SCORE=$(python graders/heuristics.py "$OUTPUT")
    LLM_SCORE=$(python graders/llm_judge.py "$INPUT" "$OUTPUT")
    
    echo "{\"case\": \"$case\", \"det\": $DET_SCORE, \"heur\": $HEUR_SCORE, \"llm\": $LLM_SCORE}" >> $RESULTS_FILE
done

# Aggregate and compare to previous run
python compare_results.py $RESULTS_FILE
```

---

### Token and cost analysis

**Simple version:** Every eval run costs money. Track how much each workflow costs to run so you can decide if the quality improvement is worth the token spend.

**Technical version:** For each eval run, capture: input tokens, output tokens, model used, cache hit rate, latency. Aggregate into cost-per-task. Compare across: different prompts for the same task, different models, different workflow architectures.

**Example — cost tracking in eval runner:**
```python
import anthropic

client = anthropic.Client()

def run_with_cost_tracking(prompt, model="claude-sonnet-4-6"):
    start = time.time()
    response = client.messages.create(
        model=model,
        max_tokens=1000,
        messages=[{"role": "user", "content": prompt}]
    )
    end = time.time()
    
    return {
        "output": response.content[0].text,
        "input_tokens": response.usage.input_tokens,
        "output_tokens": response.usage.output_tokens,
        "latency_ms": (end - start) * 1000,
        "cost_usd": (
            response.usage.input_tokens * 0.000003 +   # Sonnet 4.6 input
            response.usage.output_tokens * 0.000015    # Sonnet 4.6 output
        )
    }
```

**Cost benchmarking questions to answer:**
- What does this workflow cost per execution?
- Is Haiku acceptable quality for this task? (If yes: 5x cheaper)
- Does prompt caching reduce my costs? (For skills with long instruction blocks: yes, significantly)
- What's the quality/cost Pareto frontier across models?

---

### Anthropic's skill-creator eval (meta-eval)

The skill-creator includes a built-in eval framework specifically for testing trigger reliability — the most common failure mode for skills. It:
1. Generates test queries from your skill's description
2. Runs each query 3 times to get a reliable trigger rate
3. Calls Claude to propose description improvements based on failures
4. Re-evaluates on train + test sets to avoid overfitting
5. Iterates up to 5 times

This is the eval you run first for any new skill. Everything else comes after.

---

### Sandbox exercise

1. Install the skill-creator and build a commit message skill
2. Run its built-in eval — note the initial trigger rate
3. Read the description changes it proposes and why
4. Run again after 5 iterations — compare final trigger rate to initial

### Apply to your project

1. **Pick the skill you built in Phase 2.** Write 10 test prompts: 5 that should trigger it, 5 that shouldn't.
2. **Build a simple eval script** that runs all 10 prompts and records whether the skill fired. Store results in `notes/eval-results.json`.
3. **Add cost tracking** to your eval script using the code above.
4. **Establish a baseline score** before any changes.
5. **Run your eval every time you modify the skill.** Compare scores. Did the change help?
6. **Add a second grader:** pick one quality criterion for your skill's output (correctness, format compliance, whatever matters most) and write an LLM judge rubric for it.

**Tools:** `anthropics/skills` skill-creator eval · `promptfoo` · `anthropic/evals` cookbook

---

## Quick Reference: Phases & Repos

| Phase | What you learn | Key repos |
|---|---|---|
| 1 | Skills, personas, agents — mental models | `anthropics/skills` |
| 2 | Build and eval your first skill | `anthropics/skills` (skill-creator) · `dscv103/claude-skills-cli` |
| 2.5 | Commands — built-in and custom | Claude Code built-ins |
| 3 | Role-based personas, subagent agents | `garrytan/gstack` · `alirezarezvani/claude-skills` |
| 3.5 | Context windows, memory, handoffs | `daymade/claude-code-skills` (continue-claude-work) |
| 4 | CLAUDE.md, forced-eval, project wiring | `obra/superpowers` |
| 4.5 | Hooks — deterministic automation | `disler/claude-code-hooks-mastery` |
| 5 | Multi-agent: subagents, Agent Teams | Claude Code native |
| 5.5 | Forking — exploratory vs execution | Claude Code native (context: fork) |
| 5.75 | Git worktrees — parallel isolation | `code.claude.com/docs/en/common-workflows` |
| 6 | Full orchestration: Gas Town, oh-my-cc | `steveyegge/gastown` · oh-my-claudecode |
| 7 | Evals, graders, benchmarking, cost | `anthropics/skills` (skill-creator eval) |

---

## Your `notes/agent-plan.md`

```markdown
# Agent plan — [project name]

## Repeating tasks → skills
- [ ] [task] | skill: [name] | status: [not started / built / installed]

## Personas I need
- Reviewer: cares about [X, Y, Z]
- [Other role]: ...

## Agent candidates (completable in isolation)
- [ ] [task] | success: [...] | status: [not started / tested]

## CLAUDE.md rules added (by phase)
- P1: [project context added]
- P2: [skill routing rules]
- P3: [persona routing rules]
- P4: [conventions, forced-eval]
- P4.5: [what hooks now automate]
- P3.5: [compaction instructions, handoff rules]

## Hooks configured
- SessionStart: [what it injects]
- UserPromptSubmit: [what it adds]
- PreToolUse: [what it guards]
- PostToolUse: [what it automates]
- Stop: [what it validates]
- SubagentStop: [output validation]

## Context management
- Compaction: preserve [list]
- Handoff file: notes/session-handoff.md updated [date]
- Memory survived last session: [yes / mostly / no]

## Worktree usage
- Active worktrees: [list]
- Agents with isolation:worktree: [list]

## Eval results
| Skill | Trigger rate | Quality score | Cost/run | Date |
|---|---|---|---|---|
| [skill-name] | X/10 | X/5 | $X | [date] |

## Cost benchmarks
- Solo session on [task]: ~[tokens / $cost]
- Multi-agent on [task]: ~[tokens / $cost]
- Eval suite run: ~[cost]
- Worth it: [yes/no + reason]

## What Claude keeps getting wrong
- [mistake] → [rule added to CLAUDE.md]
```
