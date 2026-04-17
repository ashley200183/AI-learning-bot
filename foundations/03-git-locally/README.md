# F3 — Git Locally

**Time:** 15–30 min (some git exposure) · 60–90 min (first time)
**Prerequisites:** F1, F2 complete

## The mental model

**Git is a time machine for your code.** Every time you make a meaningful change, you record a *snapshot* (called a **commit**) and write a one-line note about what changed. Later you can browse history, compare versions, or roll back. That's 80% of what git does.

The magic: you can branch off at any point, try something different, and merge back if it works — or abandon it if it doesn't. More on branches in F5.

## The three states

Every file in a git repo is in one of three states:

1. **Working directory** — what's on disk right now, what you're editing
2. **Staging area** (index) — changes you've marked ready to commit, but haven't committed yet
3. **Committed** — recorded in git history, safe, has a unique ID (the commit hash)

The workflow:
```
edit file  →  git add file  →  git commit  →  (done, now in history)
 working       staging          committed
```

You can `git add` selectively — stage *some* changes now, leave others unstaged — which is why there's a middle layer.

## The commands you'll use constantly

### Setup (one time per project)

```bash
git init              # initialize a new repo in the current directory
git config user.name "Your Name"
git config user.email "you@example.com"
```

If you haven't set name/email globally yet, do it once:
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### The core loop

```bash
git status            # what's changed, what's staged, what's untracked
git diff              # show unstaged changes line-by-line
git diff --staged     # show staged changes
git add file.txt      # stage a specific file
git add .             # stage everything (be careful — includes junk)
git commit -m "msg"   # commit staged changes with a message
git log               # browse commit history
git log --oneline     # one line per commit
```

### Undoing (carefully)

```bash
git restore file.txt       # throw away unstaged changes to this file
git restore --staged file  # unstage (put back into working dir)
git commit --amend         # change the most recent commit (before pushing)
git reset --soft HEAD~1    # undo last commit, keep changes staged
git reset --hard HEAD~1    # DANGER — undo last commit, lose changes
```

`--hard` deletes work. Use it only when you're sure.

## `.gitignore` — what NOT to track

Some files shouldn't be in git: secrets, dependencies, build outputs, system files. Add them to a `.gitignore` file in the repo root:

```
# Dependencies
node_modules/
__pycache__/
.venv/

# Environment / secrets — NEVER commit
.env
.env.local
*.pem
*.key

# OS cruft
.DS_Store
Thumbs.db

# Build output
dist/
build/
*.log
```

The golden rule: **if it's a secret or can be regenerated, don't commit it.** `.env` files hold API keys and should never touch git.

## Commit messages that don't embarrass you

Not this:
```
fix stuff
wip
asdf
final final v2
```

This:
```
fix(auth): handle expired refresh token without throwing
feat(checkout): add Stripe webhook handler
docs: update README install steps
```

**Conventional Commits** format: `type(scope): short description`. Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`. This is exactly what the AI curriculum's commit-style skill (Section 2) teaches — you'll build the skill that enforces this.

## Exercise

See `exercises/01-basic/BRIEF.md`.
