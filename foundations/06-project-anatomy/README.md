# F6 — Project Anatomy & Running Code

**Time:** 45–60 min (run projects before) · 75–90 min (first time)
**Prerequisites:** F1–F5 complete

## Why this matters

Real projects have config files that tell tools how to behave, lock files that pin exact versions, and scripts you can invoke. When Claude edits these files — or when something breaks and you see a stack trace — you need to know what you're looking at. This section covers the minimum to not panic.

## Part 1 — The config files per language

Every modern language has a file at the project root that declares: what this project is called, what it depends on, how to build/run/test it. You'll see these files often; Claude will edit them often.

| Language | File | Lock file |
|---|---|---|
| Node / JavaScript / TypeScript | `package.json` | `package-lock.json` or `yarn.lock` or `pnpm-lock.yaml` |
| Python (modern) | `pyproject.toml` | `poetry.lock` or `uv.lock` |
| Python (legacy) | `requirements.txt` | — (use `pip freeze`) |
| Rust | `Cargo.toml` | `Cargo.lock` |
| Go | `go.mod` | `go.sum` |
| Ruby | `Gemfile` | `Gemfile.lock` |

### Reading a `package.json` (the one you'll see most)

```json
{
  "name": "my-app",
  "version": "1.2.0",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "test": "vitest",
    "lint": "eslint ."
  },
  "dependencies": {
    "next": "^14.1.0",
    "react": "^18.2.0"
  },
  "devDependencies": {
    "typescript": "^5.3.0",
    "eslint": "^8.50.0"
  }
}
```

Key parts:

- **`scripts`** — runnable commands. Run them with `npm run <name>` (or `yarn <name>` or `pnpm <name>`). So `npm run dev` starts the dev server.
- **`dependencies`** — libraries your *running* code needs. Ship with production.
- **`devDependencies`** — libraries only needed during development (testing, linting, build tools). Don't ship.
- **Version strings** — see the semver section below.

When you run `npm install`, npm reads `dependencies` + `devDependencies`, resolves all the transitive dependencies, downloads them to `node_modules/`, and writes the exact versions it chose to `package-lock.json`.

### Python equivalent — `pyproject.toml`

```toml
[project]
name = "my-app"
version = "1.2.0"
dependencies = [
  "fastapi>=0.100",
  "pydantic~=2.5",
]

[project.optional-dependencies]
dev = ["pytest>=7", "black>=23"]
```

Same idea: declares what's needed, how to build, what dev tools to install.

## Part 2 — Lock files: why commit them

Lock files pin the *exact* version of every dependency (direct and transitive). Without a lock file, two teammates running `npm install` might get different versions of the same library — "works on my machine" territory.

**Always commit lock files.** They go in git, not `.gitignore`. The exception: *don't commit both* `package-lock.json` AND `yarn.lock` — pick one tool and stick with it.

When Claude proposes "just delete `package-lock.json`," that's usually wrong. When Claude regenerates it via `npm install`, that's fine.

## Part 3 — Semantic versioning (semver)

Version numbers are `MAJOR.MINOR.PATCH`:

- **PATCH** (1.2.3 → 1.2.4) — bug fixes, no new behavior
- **MINOR** (1.2.3 → 1.3.0) — new features, backwards compatible
- **MAJOR** (1.2.3 → 2.0.0) — breaking changes

In config files:

| Spec | Means | Example of what it accepts |
|---|---|---|
| `1.2.3` | exactly this version | only 1.2.3 |
| `^1.2.3` | compatible within this major | 1.2.3, 1.2.9, 1.8.0 — but NOT 2.0.0 |
| `~1.2.3` | compatible within this minor | 1.2.3, 1.2.9 — but NOT 1.3.0 |
| `>=1.2.3` | at least this | 1.2.3, 2.0.0, 5.0.0 |
| `*` or `latest` | anything | anything — **avoid in production** |

The `^` caret is the default npm adds. It's usually fine, but knowing that `^1.2.3` could silently pull in 1.9.0 matters when debugging "it broke and I didn't change anything."

**When Claude bumps a dependency:** know the difference between a patch (safe), a minor (probably safe), and a major (read the changelog before you merge).

## Part 4 — Running code

### The canonical flow for any project

```bash
# 1. Install dependencies (reads config + lock file, writes node_modules/ or similar)
npm install         # Node
pip install -e .    # Python (modern editable install)
cargo build         # Rust
go mod download     # Go

# 2. Find the run command
cat package.json | grep -A 5 "scripts"   # inspect what's available
npm run dev         # or whatever the dev script is called

# 3. Run tests
npm test            # or pytest, cargo test, go test ./...
```

If you don't know how to run somebody's project: **check their README first**, then **check the `scripts` section of their config file**. If neither tells you, that's a documentation gap — file an issue or ask.

### Dev servers, ports, localhost

When you run `npm run dev` and see:
```
ready - started server on 0.0.0.0:3000, url: http://localhost:3000
```

…a program is now listening on **port 3000** on your machine. Open `http://localhost:3000` in a browser to see it. When you close the terminal process (`Ctrl+C`), the server stops.

Common ports by convention:
- `3000` — Node / Next.js / React dev servers
- `5173` — Vite
- `8000` or `8080` — Python (FastAPI, Django)
- `5432` — PostgreSQL
- `6379` — Redis

### Port conflicts

Error like `EADDRINUSE: address already in use :::3000` means something else is on that port. Options:
```bash
# macOS / Linux: find and kill the other process
lsof -i :3000              # see what's using it
kill <pid>                 # kill by PID

# Or run your server on a different port
PORT=3001 npm run dev
```

## Part 5 — Reading error messages

An error message is a gift. It's telling you exactly what broke. Learning to read it fast is a 10x skill.

### Anatomy of a JS stack trace

```
TypeError: Cannot read properties of undefined (reading 'map')
    at UserList (/app/src/components/UserList.tsx:14:22)
    at renderWithHooks (/app/node_modules/react-dom/cjs/react-dom.development.js:16305:18)
    at mountIndeterminateComponent (...)
```

Read from the top:
1. **Error type** — `TypeError`. Class of problem.
2. **Error message** — `Cannot read properties of undefined (reading 'map')`. Something you called `.map()` on was `undefined`.
3. **First line of stack trace** — `at UserList (/app/src/components/UserList.tsx:14:22)`. Your code. Line 14, column 22.
4. **Rest of the trace** — usually library internals. Ignore unless the bug is actually in the library.

**Rule of thumb:** the first stack frame inside *your* code is where to start looking.

### Anatomy of a Python traceback

```
Traceback (most recent call last):
  File "/app/main.py", line 42, in <module>
    result = process(data)
  File "/app/processor.py", line 18, in process
    return [item.upper() for item in items]
AttributeError: 'NoneType' object has no attribute 'upper'
```

Python traces go in **execution order** — the bottom is where it exploded. Read bottom-up:
1. Last frame: `/app/processor.py:18` — that's where it crashed.
2. The error: `AttributeError: 'NoneType' object has no attribute 'upper'` — one of the `items` was `None`.

### Common error patterns

| Error | Likely cause |
|---|---|
| `Module not found: X` | dependency missing — run `npm install` / `pip install` |
| `EADDRINUSE: address already in use` | port conflict — see port section above |
| `Cannot read properties of undefined` / `NoneType has no attribute` | a variable you expected to be a thing was empty |
| `SyntaxError` | typo — look at the line it cites and the one before |
| `ECONNREFUSED` | something you're trying to connect to (DB, API) isn't running |
| `401 Unauthorized` / `403 Forbidden` | auth problem — check your API key / token |

When Claude breaks something and the error is confusing: **paste the error into Claude and ask.** Pattern-matching errors is genuinely something AI is good at.

## Exercise

See `exercises/01-basic/BRIEF.md`. You'll clone a real repo, read its config, run it, break it intentionally, and read the stack trace.
