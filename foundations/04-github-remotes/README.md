# F4 — GitHub & Remotes

**Time:** 20–40 min (GitHub account exists) · 60–75 min (from scratch)
**Prerequisites:** F3 complete

## What GitHub actually is

**Git** is the version control tool. It runs locally. **GitHub** is a web service that hosts remote copies of git repos + adds collaboration features on top: pull requests (PRs), issues, CI/CD, access control, web UI for browsing code.

Alternatives exist — GitLab, Bitbucket, Gitea — all work the same way from a git perspective. This curriculum uses GitHub because it's the default and most AI tooling integrates with it.

## Local vs remote

- **Local repo:** the `.git` directory in your project folder, on your machine.
- **Remote repo:** a copy hosted somewhere else (GitHub).
- `git push` sends your commits *to* the remote.
- `git pull` brings commits *from* the remote.
- `git clone` copies a remote repo to your machine.

Multiple people can have local copies of the same repo and sync via the remote. That's what enables collaboration.

## Authentication — SSH or HTTPS

GitHub needs to know you are you when you push/pull. Two paths:

**Option 1: HTTPS with a Personal Access Token (simpler).**
- Create a PAT in GitHub Settings → Developer settings → Personal access tokens
- When git prompts for a password, paste the token (not your GitHub password)
- A credential helper will cache it

**Option 2: SSH key (what most developers use).**
- Generate a key: `ssh-keygen -t ed25519 -C "you@example.com"` (accept defaults)
- Copy the public key: `cat ~/.ssh/id_ed25519.pub`
- Add it to GitHub: Settings → SSH and GPG keys → New SSH key
- Test: `ssh -T git@github.com` → should greet you by username

SSH is the slightly-more-setup but better-long-term path.

## The `gh` CLI

GitHub's official CLI is `gh`. It lets you create repos, PRs, and issues without leaving the terminal. Install:
- macOS: `brew install gh`
- Linux: see `cli.github.com` install docs
- Windows: `winget install --id GitHub.cli`

Auth once:
```bash
gh auth login
```
Pick HTTPS or SSH, follow prompts. After this, `gh` knows who you are; git operations that need auth will also work via this.

## The commands you'll use

### Linking local → remote

```bash
# You made a repo on GitHub. Now link your local repo to it.
git remote add origin git@github.com:yourname/repo.git
git push -u origin main        # first push — -u sets upstream
```

After `-u`, subsequent pushes are just `git push`.

### Cloning (remote → new local)

```bash
git clone git@github.com:yourname/repo.git
cd repo
# you now have the full history locally
```

### Daily syncing

```bash
git pull                # fetch + merge from origin/current-branch
git push                # push current branch to origin
git fetch               # fetch without merging (lower-risk inspection)
git remote -v           # list remotes you have configured
```

### Creating a repo via `gh`

```bash
# From an existing local repo:
gh repo create myrepo --public --source=. --remote=origin --push

# Or a fresh one:
gh repo create myrepo --public --clone
cd myrepo
```

## Pull Requests — the preview

You don't need to master PRs yet. Just the shape:

1. You create a branch, make changes, push the branch to GitHub.
2. You open a **Pull Request** on GitHub — a request to merge your branch into `main` (or another branch).
3. Teammates review, discuss, request changes.
4. When approved, you **merge** the PR. GitHub combines your branch's commits into the target.
5. Delete the branch. Done.

This is how teams ship code safely: nobody commits directly to `main`; all changes go through a review gate.

In this curriculum (solo work), you'll use PRs lightly — more to practice the mechanics than because you need review.

## Exercise

See `exercises/01-basic/BRIEF.md`. You'll create a GitHub repo, push to it, make a change on a branch, and open your first PR.
