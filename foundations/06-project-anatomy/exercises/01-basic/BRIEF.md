# Exercise F6.1 — Read a Real Project, Run It, Break It

**Time:** 45–60 min (run projects before) · 75–90 min (first time)
**Grading:** pass/fail
**Dependencies:** Node.js 20+ (or another language runtime — see note below)

## Objective

Pick up a small real project, inspect its config, install dependencies, run it, break it intentionally, read the stack trace, and fix it.

## Setup — pick your language

This exercise works for any language; examples use Node because `npm` is the most universal. If you'd rather use Python, Rust, or Go, adapt the steps — the *shape* is the same.

### Install Node (if you don't have it)

```bash
# macOS
brew install node

# Linux (use nvm — install from https://github.com/nvm-sh/nvm)
nvm install --lts

# Windows
winget install OpenJS.NodeJS.LTS
```

Verify:
```bash
node --version      # v20+ preferred
npm --version
```

## Steps

Use a scratch directory:
```bash
mkdir -p ~/project-practice && cd ~/project-practice
```

### 1. Create a small real project

We'll scaffold a tiny working project you can actually run:

```bash
mkdir hello-server && cd hello-server
npm init -y                         # creates package.json with defaults
npm install express
npm install --save-dev nodemon
```

Observe: `package.json` now has both a `dependencies` entry (express) and a `devDependencies` entry (nodemon). A `node_modules/` folder was created. A `package-lock.json` was written.

### 2. Write minimal code

Create `index.js`:

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from Foundations F6!');
});

app.get('/user/:id', (req, res) => {
  const user = { id: req.params.id, name: 'Ada Lovelace' };
  res.json({ user: user, upperName: user.name.toUpperCase() });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`listening on :${PORT}`);
});
```

### 3. Add run scripts

Edit `package.json` — replace the `"scripts"` block with:

```json
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
```

### 4. Inspect the config

Answer these in `submission/inspect.md`:
- What are the dependencies? Is each one `^`, `~`, or exact?
- What scripts can you run?
- What's the size of `node_modules/` (`du -sh node_modules`)? Is that what you expected?
- Is `package-lock.json` committed via `.gitignore`? What about `node_modules/`?

Set up proper `.gitignore`:
```bash
cat > .gitignore <<EOF
node_modules/
.env
.DS_Store
*.log
EOF
```

### 5. Run the project

```bash
npm start
```

Open `http://localhost:3000` in a browser. You should see "Hello from Foundations F6!".

In a second terminal (or after stopping with Ctrl+C), try `http://localhost:3000/user/42` — you should see JSON with user info.

Stop the server when done (`Ctrl+C`).

### 6. Cause a port conflict (intentional)

Start the server in terminal 1:
```bash
npm start
```

In terminal 2, try starting it again:
```bash
npm start
```

You'll get `EADDRINUSE` error. In `submission/port-conflict.md`:
- Paste the full error
- Run `lsof -i :3000` (macOS/Linux) or `netstat -ano | findstr :3000` (Windows) and paste the output
- Kill the first server with Ctrl+C and confirm the error resolves

### 7. Break the code, read the stack trace, fix it

Intentionally introduce a bug. Edit `index.js` — change the `/user/:id` handler to:

```javascript
app.get('/user/:id', (req, res) => {
  const user = null;                                          // BUG: was an object
  res.json({ user: user, upperName: user.name.toUpperCase() });
});
```

Restart the server (`npm start`) and visit `http://localhost:3000/user/42`. Server will crash.

Copy the full stack trace into `submission/stack-trace-analysis.md`. Then, without Claude's help, answer:
- What's the error type?
- What's the error message in plain English?
- Which line in YOUR code did it crash on?
- What's the specific fix?

Apply your fix. Verify the endpoint works again.

### 8. Bump a dependency to break something (optional)

If you want more practice with semver: bump express to an older incompatible version:
```bash
npm install express@3        # intentionally very old major
npm start
```

Observe what breaks (express v3 had different APIs). Then restore:
```bash
npm install express@latest
```

Capture the experience in `submission/semver-observation.md` (optional).

## Submission

- `submission/inspect.md` — config inspection answers
- `submission/port-conflict.md` — error + lsof output
- `submission/stack-trace-analysis.md` — trace + your analysis
- `submission/semver-observation.md` — (optional) what you observed on the downgrade
- `submission/reflection.md`:
  - What does `^1.2.3` mean in a dependency spec?
  - What does "`npm run dev`" actually do (in terms of files it reads)?
  - When you read a stack trace, which frame tells you where YOUR bug is?
  - What's the difference between `dependencies` and `devDependencies`?

## Pass criteria

- [ ] Project runs — you saw "Hello from Foundations F6!" in a browser
- [ ] You reproduced and resolved a port conflict
- [ ] You caused a `TypeError`, read the trace, and fixed it correctly
- [ ] Reflection answers are specific

## Common snags

- **`node: command not found`** → Node isn't on your PATH. `which node` — if empty, reinstall or check your profile (`~/.zshrc`, `~/.bashrc`).
- **`Cannot find module 'express'`** → you didn't run `npm install`, or you ran it in the wrong directory.
- **Browser says "This site can't be reached"** → the server isn't running (check terminal) or you're visiting the wrong port.
- **Permission errors on `npm install`** → don't use `sudo`. Fix your npm prefix or use nvm.
