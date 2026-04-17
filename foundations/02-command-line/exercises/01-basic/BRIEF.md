# Exercise F2.1 — Survive in the Shell

**Time:** 20–40 min (some CLI exposure) · 60–90 min (first time)
**Grading:** pass/fail

## Objective

Prove you can navigate, create, inspect, pipe, and install from the command line.

## Steps

Work in a scratch directory — do NOT pollute the repo. Create it first:

```bash
mkdir -p ~/shell-practice && cd ~/shell-practice
pwd                   # confirm you're in shell-practice
```

### 1. Create a structure

```bash
mkdir -p notes/{meetings,ideas}
touch notes/README.md notes/meetings/2026-04-16.md notes/ideas/claude.md
ls -la notes/
ls -la notes/meetings/
```

### 2. Put content in a file and view it

```bash
echo "# My Notes" > notes/README.md
echo "This is the second line" >> notes/README.md
cat notes/README.md
head -1 notes/README.md
```

### 3. Use a pipe

```bash
ls notes/ | grep -v README   # list everything except README
```

### 4. Install a tool

Pick **ripgrep** (`rg`) — it's small, useful, and you'll want it later.

- macOS: `brew install ripgrep`
- Ubuntu/Debian: `sudo apt install ripgrep`
- Windows: `winget install BurntSushi.ripgrep.MSVC`

Verify:
```bash
rg --version
which rg
```

### 5. Use the installed tool

```bash
rg "second" notes/     # should find the line in README.md
```

### 6. Explore the learning repo with your new tools

From the learning bot repo:
```bash
cd "/path/to/AI Learning Bot"
rg -l "GATE" sections/    # list files that mention GATE in sections/
rg "Time estimate" foundations/  # find time estimates in foundations briefs
```

### 7. Clean up the scratch dir (optional but tidy)

```bash
rm -r ~/shell-practice
```

## Submission

Save to `submission/terminal-log.md` a copy/paste of your terminal session for all the steps above. Format:

````markdown
## Step 1
```
$ mkdir -p ~/shell-practice && cd ~/shell-practice
$ pwd
/Users/yourname/shell-practice
```

## Step 2
...
````

Plus `submission/reflection.md` (short — 4 bullets):
- One command you'd never used before
- One command that behaved differently than you expected
- What `|` (pipe) does, in your own words
- What `PATH` is, in one sentence

## Pass criteria

- [ ] All 6 active steps ran without error (step 7 is optional)
- [ ] Installed tool works (`rg --version` prints)
- [ ] You can articulate what a pipe does and what PATH is

## Common snags

- **"command not found: brew"** → install Homebrew first (see F2 README)
- **"permission denied"** when using `apt` → prefix with `sudo`
- **`rg` installed but not found** → open a fresh terminal; PATH updates don't apply to your current shell until restart
- **Can't `cd` to the repo because path has a space** → quote it: `cd "/path/with space/repo"`
