# Exercise F4.1 — Your First GitHub Repo + PR

**Time:** 20–40 min (GitHub account exists) · 60–75 min (from scratch)
**Grading:** pass/fail
**Dependencies:** a free GitHub account (sign up at github.com if needed)

## Objective

Create a GitHub repo from scratch, push to it, make a change on a branch, open and merge a PR — all from the command line.

## Setup

### 1. Install `gh` (GitHub CLI)

- macOS: `brew install gh`
- Linux: see [cli.github.com](https://cli.github.com)
- Windows: `winget install --id GitHub.cli`

Verify: `gh --version`

### 2. Authenticate

```bash
gh auth login
```

Choose GitHub.com → HTTPS (easier for a first pass; you can switch to SSH later) → login via web browser.

Verify:
```bash
gh auth status       # should show you're logged in
```

## Steps

### 3. Create a fresh local repo

```bash
mkdir -p ~/github-practice && cd ~/github-practice
git init
echo "# GitHub Practice" > README.md
git add README.md
git commit -m "docs: initial commit"
```

### 4. Create a remote repo + push

```bash
gh repo create github-practice --public --source=. --remote=origin --push
```

This:
- Creates `yourname/github-practice` on GitHub
- Adds it as your `origin` remote
- Pushes your current branch

Verify:
```bash
gh browse            # opens the repo in your browser
git remote -v        # shows origin pointing to GitHub
```

### 5. Make a second commit and push

```bash
echo "This repo exists for learning." >> README.md
git add README.md
git commit -m "docs: add description"
git push
```

Refresh the browser tab — your change should appear.

### 6. Make a change on a branch (preview of F5)

```bash
git checkout -b add-todo
echo "- [ ] practice more git" > TODO.md
git add TODO.md
git commit -m "docs: add TODO list"
git push -u origin add-todo
```

### 7. Open a Pull Request

```bash
gh pr create --title "Add TODO list" --body "Starting a TODO for future items."
```

This opens a PR from `add-todo` → `main`. `gh` will print a URL. Open it in your browser to see the PR page.

### 8. Merge the PR from the command line

```bash
gh pr merge --merge       # or --squash or --rebase
git checkout main
git pull                  # bring the merged commit down locally
git branch -d add-todo    # delete the local branch
```

Verify: `git log --oneline` shows the merged commit on `main`.

## Submission

Save to `submission/`:

- `submission/repo-url.txt` — the URL of your GitHub repo (e.g., `https://github.com/yourname/github-practice`)
- `submission/screenshots/` — at least two:
  - The PR page showing "Merged"
  - The repo's commit history on GitHub showing ≥3 commits
- `submission/terminal-log.md` — paste of the commands you ran and their output
- `submission/reflection.md`:
  - Difference between `git push` and `git pull` in one sentence each
  - What a remote actually is (storage? copy? both?)
  - Why teams require PRs even for small changes

## Pass criteria

- [ ] Public GitHub repo exists at a URL you can share
- [ ] ≥3 commits in history visible on GitHub
- [ ] One PR was opened and merged
- [ ] Local `main` is synced with remote after the merge

## Common snags

- **`gh` asks for a token / auth fails** → run `gh auth login` again, follow the browser flow
- **`permission denied (publickey)`** on push → you chose SSH in `gh auth login` but don't have a key configured. Either redo with HTTPS, or set up SSH keys (see F4 README)
- **Push fails: "remote contains work you don't have"** → someone pushed before you (not likely for your own repo). Run `git pull --rebase` then `git push`.
- **Accidentally committed a secret** → don't just delete the file in the next commit; the secret is in git history. Rotate the secret. For small repos you can nuke history with `git filter-repo` but it's out of scope here.
