# F2 — Command Line Fundamentals

**Time:** 20–40 min (some CLI exposure) · 60–90 min (first time)
**Prerequisites:** F1 complete

## Why this matters

AI agents run commands in a terminal. If you don't have mental models for `cd`, `ls`, pipes, and PATH, you won't be able to read what Claude is doing — let alone fix it when something breaks. You don't need to *memorize* commands; you need to know what class of thing each one is so you can Google the right phrase or ask Claude the right question.

## The essentials

### Navigation

```bash
pwd           # print working directory (where am I?)
ls            # list files in current directory
ls -la        # long format, includes hidden files (those starting with .)
cd path       # change directory to path
cd ..         # up one level
cd ~          # home directory
cd -          # previous directory
```

Absolute paths start with `/` (Unix) or `C:\` (Windows). Relative paths are from wherever you are now.

### Files and directories

```bash
mkdir myfolder            # make a directory
mkdir -p a/b/c            # make nested dirs, create parents as needed
touch file.txt            # create an empty file (or update its timestamp)
cp source dest            # copy
cp -r src/ dst/           # copy directory recursively
mv old new                # move or rename
rm file.txt               # remove a file
rm -r folder/             # remove a folder (DANGER — no undo)
```

**Critical safety rule:** `rm -rf` with a wrong path can wipe your hard drive. Never run it with a variable or a wildcard without checking what it expands to. Claude's hooks in the AI curriculum (Section 9) exist precisely because of this class of accident.

### Viewing files

```bash
cat file.txt              # dump the whole file
less file.txt             # scroll through (press q to quit)
head -20 file.txt         # first 20 lines
tail -20 file.txt         # last 20 lines
tail -f log.txt           # watch a growing log (Ctrl+C to stop)
```

### Pipes and redirects

```bash
command | another         # pipe output of one into input of another
ls | grep foo             # list files, filter to those containing "foo"

command > file.txt        # redirect output to file (overwrites)
command >> file.txt       # append to file
command 2> errors.txt     # redirect errors (stderr) separately
```

### What's installed: PATH and `which`

When you type `git`, the shell searches a list of directories (your **PATH**) for an executable called `git`. First match wins.

```bash
which git                 # shows the path to the git executable
echo $PATH                # shows your PATH as colon-separated dirs
```

If a command "isn't found," either it's not installed, or it's installed somewhere not on your PATH.

### Installing tools — package managers

| OS | Tool | Example |
|---|---|---|
| macOS | **Homebrew** (`brew`) | `brew install ripgrep` |
| Linux (Debian/Ubuntu) | `apt` | `sudo apt install ripgrep` |
| Linux (Arch) | `pacman` | `sudo pacman -S ripgrep` |
| Windows | `winget` or `choco` | `winget install BurntSushi.ripgrep.MSVC` |
| Cross-platform | `npm` | `npm install -g some-tool` (for Node tools) |
| Cross-platform | `pip` | `pip install some-tool` (for Python tools) |

Install Homebrew first on macOS if you don't have it: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

## Useful commands worth knowing

| Command | What it does |
|---|---|
| `grep pattern file` | search file contents |
| `ripgrep` / `rg pattern` | faster grep, respects .gitignore |
| `find . -name "*.md"` | find files matching a pattern |
| `curl url` | download/fetch a URL |
| `ps aux \| grep node` | list running processes, filter |
| `kill <pid>` | stop a process by its ID |
| `Ctrl+C` | interrupt current command |
| `Ctrl+D` | send EOF (exit many prompts) |
| `Ctrl+R` | search your command history |

## Exercise

See `exercises/01-basic/BRIEF.md`. You'll install a tool, practice navigation, use pipes, and prove you can survive in a shell.
