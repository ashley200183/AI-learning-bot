# Exercise F5.1 — Branch, Worktree, Environment Config

**Time:** 30–45 min (used branches before) · 60–90 min (first time)
**Grading:** pass/fail
**Dependencies:** the `github-practice` repo from F4.1

## Objective

Work on a real branch, spin up a worktree to fix "something urgent" in parallel, set up `.env` / `.env.example` convention, and clean up after yourself.

## Part A — Branch work

### 1. Start clean

```bash
cd ~/github-practice
git checkout main
git pull
```

### 2. Create and work on a feature branch

```bash
git checkout -b feature/add-contact-info
echo "Contact: me@example.com" > CONTACT.md
git add CONTACT.md
git commit -m "feat: add contact info"
echo "Also reachable on Bluesky." >> CONTACT.md
git add CONTACT.md
git commit -m "feat(contact): add bluesky note"
git push -u origin feature/add-contact-info
```

### 3. Leave it mid-flight (important — do NOT merge yet)

This branch is your "in-progress feature." You'll come back to it in Part C.

## Part B — Worktree for an "urgent fix"

### 4. Spin up a worktree on a new branch

Without switching branches in your current directory:
```bash
git worktree add ../github-practice-hotfix -b fix/readme-typo
git worktree list
```

You should see two worktrees listed.

### 5. Work in the new worktree

```bash
cd ../github-practice-hotfix
# You're on fix/readme-typo, main is still at the other dir. feature/add-contact-info untouched.
sed -i.bak 's/GitHub Practice/GitHub Practice Repo/' README.md && rm README.md.bak
# (If sed syntax gives you trouble on macOS, just edit README.md in your editor.)
git add README.md
git commit -m "fix(readme): expand title"
git push -u origin fix/readme-typo
gh pr create --title "Fix README title" --body "Expand title slightly."
gh pr merge --merge --delete-branch
```

### 6. Clean up the hotfix worktree

```bash
cd ~/github-practice                      # back to original worktree
git worktree remove ../github-practice-hotfix
git worktree list                         # only one worktree now
```

## Part C — Finish the feature

### 7. Sync main (the hotfix is on there now), then rebase your feature

```bash
git checkout main
git pull                                  # brings down the hotfix merge
git checkout feature/add-contact-info
git rebase main                           # replay your commits on top of new main
git push --force-with-lease               # --force-with-lease is the safe force-push
```

If `--force-with-lease` is new to you: normal `--force` is dangerous because it blindly overwrites the remote. `--force-with-lease` refuses if the remote has changed in a way you didn't expect — so it's the safer default when you must force.

### 8. Merge the feature PR

```bash
gh pr create --title "Add contact info" --body "Adds CONTACT.md"
gh pr merge --merge --delete-branch
git checkout main
git pull
git branch -d feature/add-contact-info
```

## Part D — `.env` convention

### 9. Set up environment file pattern

```bash
cat > .env.example <<EOF
# Copy this file to .env and fill in real values
# .env is in .gitignore and should NEVER be committed

DATABASE_URL=postgres://localhost:5432/myapp_dev
API_KEY=your-key-here
LOG_LEVEL=debug
EOF

# Make a real .env (don't commit)
cp .env.example .env

# Make sure .env is in .gitignore
grep -q "^.env$" .gitignore || echo ".env" >> .gitignore

git add .env.example .gitignore
git commit -m "chore: add env file convention"
git push

# Verify .env is NOT tracked
git status                                # should be clean
git ls-files | grep .env                  # should ONLY show .env.example and .gitignore, NOT .env
```

## Submission

Save to `submission/`:

- `submission/repo-url.txt` — same repo as F4, now with branches and merged PRs
- `submission/git-log.txt` — `git log --oneline --all --graph` output showing the branch history
- `submission/worktree-list.txt` — screenshot or text of `git worktree list` mid-exercise (when both worktrees existed)
- `submission/env-example-check.txt` — output of `git ls-files | grep .env` showing `.env` is NOT tracked
- `submission/reflection.md`:
  - Why would you reach for a worktree vs stashing?
  - What's the difference between `--force` and `--force-with-lease`?
  - One real scenario in your life/work where you'd want separate dev/staging/prod environments
  - What specifically is in `.gitignore` (list 3 types of things) and why each

## Pass criteria

- [ ] Both branches (`feature/add-contact-info` and `fix/readme-typo`) existed and were merged via PR
- [ ] Used a worktree — evidence in `git worktree list` output
- [ ] Git history shows both branches' commits on `main` after merge
- [ ] `.env.example` is committed; `.env` exists locally but NOT in git
- [ ] Reflection answers are specific

## You're done with foundations

After this, you're ready for the AI curriculum. Start Section 1 (Mental Models + When NOT to Use Agents).

## Common snags

- **`git rebase main` has conflicts** → unlikely here, but if it happens: `git status` shows conflicted files, edit to resolve, `git add`, `git rebase --continue`.
- **`gh pr merge` fails with "required status checks"** → your repo has branch protection. For a practice repo, go to GitHub Settings → Branches → remove protection rules, or just merge via the GitHub web UI.
- **Worktree won't delete** — "contains modified or untracked files"** → either commit/discard the changes in that worktree first, or `git worktree remove --force`.
- **`.env` still shows in `git status`** → you committed it before adding to gitignore. Run `git rm --cached .env` then commit.
