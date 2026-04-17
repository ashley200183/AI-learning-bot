# Exercise F3.1 — Make Your First Repo

**Time:** 15–30 min (some git exposure) · 60–90 min (first time)
**Grading:** pass/fail

## Objective

Create a local git repo, make real commits, view history, use `.gitignore`, and try one undo operation.

## Steps

Work in a new scratch directory (don't touch the learning bot repo for this):

```bash
mkdir -p ~/git-practice && cd ~/git-practice
```

### 1. Initialize

```bash
git init
git status                    # should say "On branch main" (or "master" on older git)
```

If it says "master," set the default branch name for future repos:
```bash
git config --global init.defaultBranch main
```

### 2. Configure identity (if you haven't)

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### 3. Make your first commit

```bash
echo "# My Practice Repo" > README.md
git status                    # README.md is untracked
git add README.md
git status                    # now staged
git commit -m "docs: add README"
git log --oneline             # one commit now
```

### 4. Make 2 more commits

```bash
mkdir src
echo "print('hello')" > src/hello.py
git add src/hello.py
git commit -m "feat: add hello script"

echo "print('hello, world')" > src/hello.py
git status                    # modified, unstaged
git diff                      # shows the change
git add src/hello.py
git commit -m "feat(hello): add world suffix"

git log --oneline             # 3 commits
```

### 5. Add a `.gitignore`

```bash
cat > .gitignore <<EOF
.env
.DS_Store
__pycache__/
*.log
EOF

# Create a file that *would* have been committed but shouldn't be
echo "SECRET_KEY=hunter2" > .env

git status                    # .gitignore is untracked; .env is ignored (good)
git add .gitignore
git commit -m "chore: add gitignore"
git status                    # clean — .env is invisible to git
```

### 6. Try an undo — then undo the undo

```bash
echo "oops a mistake" >> src/hello.py
git diff                      # see the mistake
git restore src/hello.py      # throw it away
cat src/hello.py              # the "oops" line is gone
```

### 7. Look at history properly

```bash
git log
git log --oneline --all
git log -p -1                 # show the diff of the most recent commit
```

## Submission

Save to `submission/`:

- `submission/git-log.txt` — output of `git log --oneline` from your practice repo
- `submission/terminal-log.md` — copy/paste of your terminal session, including the output of `git status` before + after each commit
- `submission/reflection.md` — short answers (1–2 sentences each):
  - Difference between "unstaged", "staged", and "committed"
  - Why `.env` belongs in `.gitignore`
  - What `git restore file` does (vs `git reset --hard`)
  - What's wrong with the commit message `"fix stuff"`

## Pass criteria

- [ ] At least 4 commits in your practice repo
- [ ] `.gitignore` working — `.env` doesn't show up in `git status`
- [ ] You used `git restore` successfully
- [ ] Commit messages use Conventional Commits format
- [ ] Reflection answers are specific, not generic

## Common snags

- **"Please tell me who you are"** on commit → you skipped step 2 (`git config user.name` / `user.email`)
- **`.env` still shows in `git status`** → make sure you ran `git add .gitignore` and committed it. Also: if `.env` was already staged before you added it to `.gitignore`, run `git rm --cached .env` first.
- **Command edits file but `git status` shows nothing** → you edited the wrong file. Check `pwd` and `ls`.
