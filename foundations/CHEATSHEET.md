# Command Line Cheatsheet

## Navigation

| Command | Stands for | What it does |
|---|---|---|
| `pwd` | **print working directory** | shows where you are right now |
| `ls` | **list** | shows files/folders in current directory |
| `ls -l` | list **long** | shows files with size, date, permissions |
| `ls -a` | list **all** | includes hidden files (starting with `.`) |
| `ls -la` | list **long all** | both combined |
| `cd path` | **change directory** | move into a folder |
| `cd ..` | change directory **up** | go up one level (parent) |
| `cd ~` | change directory **home** | go to your home directory |
| `cd -` | change directory **back** | toggle to the last directory you were in |

## Files & Directories

| Command | Stands for | What it does |
|---|---|---|
| `mkdir name` | **make directory** | create a folder |
| `mkdir -p a/b/c` | make directory **parents** | create nested folders, no error if exists |
| `touch file` | **touch** | create empty file (or update its timestamp) |
| `cp src dest` | **copy** | copy a file |
| `cp -r src/ dest/` | copy **recursive** | copy a folder and everything inside |
| `mv old new` | **move** | move or rename a file/folder |
| `rm file` | **remove** | delete a file (no undo, no trash) |
| `rm -r folder/` | remove **recursive** | delete a folder and everything inside |
| `rm -rf folder/` | remove **recursive force** | same but no confirmation prompts. DANGER. |
| `rmdir folder` | **remove directory** | delete an empty folder only |

## Viewing Files

| Command | Stands for | What it does |
|---|---|---|
| `cat file` | **concatenate** | dump entire file to terminal |
| `head -N file` | **head** | show first N lines |
| `tail -N file` | **tail** | show last N lines |
| `tail -f file` | tail **follow** | watch a file grow in real-time (Ctrl+C to stop) |
| `less file` | **less** | scroll through a file (press `q` to quit) |

## Searching

| Command | Stands for | What it does |
|---|---|---|
| `grep pattern file` | **Global Regular Expression Print** | search file contents for a pattern |
| `grep -v pattern` | grep **invert** | show lines NOT matching the pattern |
| `grep -i pattern` | grep **insensitive** | case-insensitive search |
| `rg pattern dir/` | **ripgrep** | faster grep, respects .gitignore |
| `rg -l pattern` | ripgrep **list** | show only filenames, not matching lines |
| `find . -name "*.md"` | **find** | find files by name pattern |
| `which command` | **which** | show the full path to an executable |

## Pipes, Redirects & Chaining

| Symbol | Name | What it does |
|---|---|---|
| `\|` | **pipe** | send left command's output as input to right command |
| `>` | **redirect** | write output to a file (overwrites) |
| `>>` | **append redirect** | add output to end of file (keeps existing) |
| `&&` | **and then** | run right command only if left command succeeded |
| `;` | **semicolon** | run right command regardless of left's result |
| `2>` | **stderr redirect** | redirect error output separately |

## Output & Variables

| Command | Stands for | What it does |
|---|---|---|
| `echo text` | **echo** | print text to terminal |
| `echo $VAR` | echo **variable** | print the value of an environment variable |
| `echo $PATH` | echo **PATH** | show the directories your shell searches for commands |
| `export VAR=value` | **export** | set an environment variable for this session |

## Installing Things

| Tool | OS | Example |
|---|---|---|
| `brew install` | macOS (Homebrew) | `brew install ripgrep` |
| `brew reinstall` | macOS | `brew reinstall simdjson` (fixes broken libs) |
| `apt install` | Linux (Debian/Ubuntu) | `sudo apt install ripgrep` |
| `npm install` | Node.js packages | `npm install express` |
| `pip install` | Python packages | `pip install fastapi` |

## Process Control

| Command | What it does |
|---|---|
| `Ctrl+C` | interrupt/stop the current command |
| `Ctrl+D` | send EOF (exit many prompts/shells) |
| `Ctrl+R` | search your command history (type to filter, Enter to run) |
| `lsof -i :3000` | **list open files** on port 3000 (find what's using a port) |
| `kill PID` | stop a process by its ID |

## Git (local)

| Command | Stands for | What it does |
|---|---|---|
| `git init` | **initialize** | create a new repo in current directory |
| `git status` | **status** | show what's changed, staged, untracked |
| `git add file` | **add** | stage a file (mark it for next commit) |
| `git add .` | add **all** | stage everything (be careful) |
| `git commit -m "msg"` | **commit message** | save staged changes as a snapshot |
| `git log` | **log** | show commit history |
| `git log --oneline` | log **one line** | compact history view |
| `git diff` | **difference** | show unstaged changes line-by-line |
| `git diff --staged` | diff **staged** | show staged changes |
| `git restore file` | **restore** | throw away unstaged changes to a file |
| `git restore --staged file` | restore **staged** | unstage a file (keep the changes on disk) |

## Git (remote / GitHub)

| Command | Stands for | What it does |
|---|---|---|
| `git clone url` | **clone** | copy a remote repo to your machine |
| `git remote add origin url` | **remote add** | link your local repo to a remote |
| `git push` | **push** | send your commits to the remote |
| `git push -u origin main` | push **upstream** | first push; sets tracking so future pushes are just `git push` |
| `git pull` | **pull** | fetch + merge from remote |
| `git fetch` | **fetch** | download remote changes without merging |

## Git (branching)

| Command | Stands for | What it does |
|---|---|---|
| `git branch` | **branch** | list branches (current has a `*`) |
| `git branch name` | branch **create** | create a branch (doesn't switch to it) |
| `git checkout -b name` | checkout **branch** | create AND switch to a new branch |
| `git switch name` | **switch** | switch to an existing branch |
| `git switch -c name` | switch **create** | create and switch (modern alternative to checkout -b) |
| `git merge branch` | **merge** | merge another branch into current |
| `git branch -d name` | branch **delete** | delete a merged branch |
| `git worktree add path -b name` | **worktree add** | create a second working directory on a new branch |
| `git worktree list` | worktree **list** | show all worktrees |
| `git worktree remove path` | worktree **remove** | delete a worktree |

## GitHub CLI

| Command | What it does |
|---|---|
| `gh auth login` | authenticate with GitHub |
| `gh repo create name --public` | create a new repo on GitHub |
| `gh pr create --title "..." --body "..."` | open a pull request |
| `gh pr merge` | merge a pull request |
| `gh browse` | open the repo in your browser |

## Symbols & Wildcards

| Symbol | Name | What it does |
|---|---|---|
| `~` | **tilde** | shortcut for your home directory |
| `.` | **dot** | current directory |
| `..` | **double dot** | parent directory |
| `*` | **wildcard/glob** | matches anything (`*.md` = all markdown files) |
| `$` | **dollar** | access a variable's value (`$PATH`, `$HOME`) |
| `#` | **hash/comment** | everything after is ignored by the shell |
| `\` | **backslash** | escape the next character (treat it literally) |
| `{}` | **braces** | expansion (`{a,b}` becomes `a b`) |
