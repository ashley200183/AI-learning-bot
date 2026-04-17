# F1 — Your Dev Environment

**Time:** 15–30 min (tools pre-installed) · 45–60 min (from scratch)

## The two tools you'll always have open

### IDE (editor)

An **IDE** (Integrated Development Environment) is the window where you read and write code. It shows your project's folder tree, opens files in tabs, highlights syntax, and — for AI-enabled IDEs — hosts Claude or other assistants.

For this curriculum, use one of:
- **VS Code** — free, universal, what most tutorials assume. Good default.
- **Cursor** — VS Code fork with AI built in. Popular for vibe coding.
- **Claude Code (standalone or in VS Code)** — the Anthropic CLI. Required for the AI curriculum starting at Section 1.

You can have more than one installed. Most vibe coders use VS Code or Cursor as the editor + Claude Code inside its terminal.

### Terminal (shell)

A **terminal** is where you type commands directly to your operating system. When a tutorial says "run `npm install`," that's a terminal command. The terminal doesn't know about your code's meaning — it runs commands against your filesystem, network, and installed programs.

On macOS the default shell is **zsh**. On Linux it's usually **bash**. On Windows you'll use **PowerShell** or **WSL** (Windows Subsystem for Linux, which gives you bash).

### IDE vs terminal — when to use which

| Task | IDE | Terminal |
|---|---|---|
| Read or edit a file's contents | ✓ | (possible but awkward) |
| Run a server or install packages | (via integrated terminal) | ✓ |
| Visualize folder structure | ✓ | (via `ls`, `tree`) |
| Search across many files | ✓ (fast) | ✓ (`grep`, `rg`) |
| Git operations | ✓ (GUI plugin) | ✓ (preferred — more control) |
| Running one-off commands | — | ✓ |
| AI chat with Claude Code | ✓ (integrated) | ✓ (CLI) |

**In practice:** keep both open. Your IDE is your editing surface. Your terminal is how you run, install, and inspect.

### Integrated terminal

VS Code, Cursor, and most IDEs include a terminal *inside the IDE window* — same directory as your project, always one keystroke away. This is usually where you'll work. Open it with `Ctrl+\`` (Windows/Linux) or `Cmd+\`` (macOS).

## What you need installed for this curriculum

- **VS Code** (or Cursor)
- **Claude Code CLI** — [claude.com/code](https://claude.com/code) has install instructions for your OS
- **Git** — pre-installed on macOS/Linux; download from git-scm.com for Windows
- **A modern shell:** zsh (macOS default), bash (Linux default), or PowerShell/WSL (Windows)

## Exercise

See `exercises/01-basic/BRIEF.md`. You'll install the tools, open the repo, and verify everything works together.
