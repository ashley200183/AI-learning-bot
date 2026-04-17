# Exercise F1.1 — Set Up Your Dev Environment

**Time:** 15–30 min (tools pre-installed) · 45–60 min (from scratch)
**Grading:** pass/fail (did you complete each step and show proof)

## Steps

### 1. Install VS Code (or Cursor)

- Download from [code.visualstudio.com](https://code.visualstudio.com) (or [cursor.sh](https://cursor.sh))
- Open it. Close the welcome tab.

### 2. Install Claude Code

Follow the current install instructions at [claude.com/code](https://claude.com/code). On macOS, typically:
```bash
brew install claude-code
# or: curl -fsSL https://claude.ai/install.sh | sh
```

**Verify:** in any terminal, run `claude --version`. You should see a version number.

### 3. Install Git (if not already)

- macOS / Linux: run `git --version` in a terminal. If it prints a version, you're done. If not: macOS will prompt to install command line tools; Linux users run `sudo apt install git` or equivalent.
- Windows: download from [git-scm.com](https://git-scm.com) or use `winget install --id Git.Git`

**Verify:** `git --version` prints a version.

### 4. Open this repo in VS Code

```bash
code "/path/to/AI Learning Bot"
# Or: File → Open Folder → select the repo
```

### 5. Open the integrated terminal

- **VS Code:** `Ctrl+\`` (Windows/Linux) or `` Cmd+`  `` (macOS)
- Verify it shows the repo path in its prompt

### 6. Run three commands in the integrated terminal

```bash
pwd         # prints your current directory — should be the repo path
ls          # lists files — you should see CLAUDE.md, MAP.md, sections/, etc.
git status  # shows clean repo state (or untracked files if you've been poking)
```

### 7. Start Claude Code

In the integrated terminal:
```bash
claude
```

You should drop into a Claude Code session. Exit with `/exit` when you've confirmed it works.

## Submission

Save to `submission/`:

- `submission/screenshots/` — one screenshot showing your IDE with the integrated terminal open, showing the output of `pwd && ls && git status`
- `submission/versions.txt` — paste the output of:
  ```bash
  echo "--- vscode ---"
  code --version
  echo "--- claude ---"
  claude --version
  echo "--- git ---"
  git --version
  echo "--- shell ---"
  echo $SHELL
  ```

## Pass criteria

- [ ] VS Code / Cursor installed and opens
- [ ] Claude Code installed — `claude --version` works
- [ ] Git installed — `git --version` works
- [ ] Repo opens in IDE
- [ ] Integrated terminal works, shows repo path
- [ ] Claude Code session starts when you run `claude`

If any step fails, troubleshoot before moving on. Installation breakage here is the #1 reason learners stall in Section 1 of the AI curriculum.
