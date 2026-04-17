# F5 — Branches, Worktrees & Environments

**Time:** 30–45 min (used branches before) · 60–90 min (first time)
**Prerequisites:** F3, F4 complete

## Part 1 — Branches

### What a branch is

A **branch** is a separate line of development in your repo. Think of it as a bookmark pointing to a specific commit, with permission to add more commits without affecting the main timeline.

`main` is your default branch — the "current truth" version of the code. Feature branches are temporary off-shoots where you try something without risking `main`. When the feature works, you merge the branch back into `main`.

### Why bother?

- **Safety.** Broken code in a feature branch doesn't break `main`.
- **Parallelism.** You can work on multiple things at once, one branch each.
- **Review.** PRs require a branch — teams use them to review code before it lands.
- **Deployment.** Production usually tracks `main`. If `main` always works, prod always works.

### The commands

```bash
git branch                        # list branches; current has a *
git branch -a                     # include remote branches
git branch new-feature            # create a branch (doesn't switch)
git checkout new-feature          # switch to it
git checkout -b new-feature       # create AND switch (common shortcut)
# Modern equivalents:
git switch new-feature            # switch to existing
git switch -c new-feature         # create and switch

git merge other-branch            # merge other-branch into current
git branch -d old-branch          # delete a merged branch
git branch -D old-branch          # force-delete an unmerged branch (DANGER)
```

### Naming conventions

Loose but consistent:
- `feature/add-auth` — new capability
- `fix/expired-token` — bug fix
- `refactor/extract-user-service` — internal cleanup
- `chore/bump-deps` — routine maintenance

Slashes are just part of the name; git treats them like any character.

### The workflow

```bash
# Start from main, up to date
git checkout main
git pull

# Create a feature branch
git checkout -b feature/add-search

# Work, commit, commit, commit
# ...

# Push it up
git push -u origin feature/add-search

# Open PR (as in F4)
gh pr create

# After merge, clean up
git checkout main
git pull
git branch -d feature/add-search
```

## Part 2 — Worktrees

### Problem

You're mid-flow on `feature/payments` with 50 uncommitted changes. A critical bug report arrives. You need to fix `main`. Options:
- **Stash everything** — messy, loses context, you'll forget to unstash
- **Commit WIP** — dirty history
- **Ignore the bug** — bad

None is good.

### Solution: worktrees

`git worktree add` creates a **second working directory** tied to the same repo but on a different branch. Each worktree:
- Lives in its own folder on disk
- Has its own checked-out branch
- Shares the repo's history (no duplicate `.git` data)

Now you can fix the bug in one worktree while `feature/payments` stays untouched in another.

### The commands

```bash
git worktree add ../hotfix-auth -b fix/auth-crash    # create new worktree + branch
git worktree list                                     # see all worktrees
git worktree remove ../hotfix-auth                    # delete when done
```

The worktree folder can be anywhere on disk. A common convention: sibling directories (`../project-hotfix`) or a `.worktrees/` subfolder.

### Why this matters for vibe coding

Claude Code v2.1.49+ has first-class worktree support:
```bash
claude --worktree feature-payments    # start Claude in an isolated worktree
```

Two Claude sessions can run in parallel on two different branches, each unaware of the other, each with its own context. This is what makes parallel AI agents safe — covered in depth in AI Section 13.

## Part 3 — Environments (dev / staging / prod)

### The concept

Same codebase, different deployments:
- **Development (dev)** — your local machine while you're coding. Breaks often. No real users. Uses test data.
- **Staging** — a shared environment that mirrors production. Used to test changes before they go live. Real-enough data.
- **Production (prod)** — the real website/app. Real users, real money, real consequences. Breakage = outage.

Same code runs in all three, configured differently by environment variables.

### The typical branch → environment mapping

| Branch | Deploys to | Touched by |
|---|---|---|
| `main` (or `production`) | Production | Only merged, reviewed PRs |
| `develop` (or `staging`) | Staging | Completed features awaiting prod |
| `feature/*` | Dev / preview URLs | Individual developers |

Modern platforms (Vercel, Netlify, Render, Fly.io) often auto-deploy:
- Every branch → a preview URL
- `main` → production

Not every project uses all three. A solo project might skip staging. A regulated enterprise might have four+ environments.

### Configuration per environment — `.env` files

Same code, different config (DB URL, API keys, logging level). Typical pattern:

```
.env                   # local dev (NOT committed; in .gitignore)
.env.example           # template with fake values (committed — so others know what vars are needed)
```

In your code:
```bash
# The app reads these at runtime
DATABASE_URL=postgres://localhost/myapp_dev
STRIPE_KEY=sk_test_xxx               # dev uses test keys
LOG_LEVEL=debug
```

In staging and prod, the same variable names are set to different values via the hosting platform's dashboard or CI secrets — never hardcoded in code, never committed.

### The golden rules

1. **Never commit `.env`** — it has secrets. It's in `.gitignore` for a reason.
2. **Never test destructive changes against prod.** If you want to see what happens when the DB is wiped, do it in dev or a throwaway staging.
3. **`main` should always be deployable.** If it's not, you or a teammate broke it and the top priority is fixing it.
4. **Production keys are different from dev keys.** A dev Stripe key charging a test card is fine; charging with it against prod is a security review failing.

## Exercise

See `exercises/01-basic/BRIEF.md`. You'll use a branch + a worktree + set up a `.env.example` pattern.
