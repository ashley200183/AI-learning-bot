# Exercise F7.1 — Inspect a Monorepo, Build a Mini One

**Time:** 30–45 min (seen monorepos) · 60–75 min (first time)
**Grading:** pass/fail
**Dependencies:** git; Node.js 20+; pnpm (install below)

## Objective

Read an open-source monorepo well enough to identify its packages and their relationships. Then scaffold a minimal monorepo of your own using pnpm workspaces.

## Setup — install pnpm

```bash
# macOS / Linux
npm install -g pnpm

# Or via corepack (newer Node)
corepack enable
corepack prepare pnpm@latest --activate
```

Verify: `pnpm --version`

## Part A — Inspect a real monorepo

Pick one of these open-source monorepos to clone and read (don't need to run it, just inspect):

- **Turborepo basic example** — `git clone https://github.com/vercel/turborepo.git && cd turborepo/examples/basic`
- **Next.js repo** — `git clone --depth 1 https://github.com/vercel/next.js.git`
- **Radix UI** — `git clone --depth 1 https://github.com/radix-ui/primitives.git`
- **Your choice** — any well-known monorepo (tRPC, Remix, Astro, etc.)

Use `--depth 1` to avoid cloning years of history. If the repo is huge, just one of these examples is fine.

In `submission/inspection.md`, answer:

1. **Workspace config.** Find the root `package.json` or `pnpm-workspace.yaml`. Where does it declare its workspaces? (Usually in `workspaces: ["..."]` or `packages: ["..."]`.)
2. **Package inventory.** List every package under `packages/` and `apps/`. For each: one-sentence what it does (from its README or `package.json` description).
3. **Dependencies between packages.** Pick two packages. Look at one's `package.json` dependencies. Does it depend on another package in the monorepo? If so, how is the dependency referenced (typically something like `"workspace:*"` or `"workspace:^"`).
4. **Build/test commands.** What's the command to build everything? Test everything? (Usually in the root `package.json`'s `scripts` or in `turbo.json`.)
5. **Shape question.** Is this a "polyrepo-in-one-dir" (packages don't share code) or a true monorepo (packages depend on each other)? Evidence?

## Part B — Build a minimal monorepo

Scratch directory:
```bash
mkdir -p ~/mini-monorepo && cd ~/mini-monorepo
```

### 1. Root setup

```bash
pnpm init              # creates root package.json
```

Edit `package.json` to make it the root of a workspace. Add:

```json
{
  "name": "mini-monorepo",
  "private": true,
  "version": "0.0.0"
}
```

Create `pnpm-workspace.yaml`:

```yaml
packages:
  - "packages/*"
  - "apps/*"
```

Create directories:
```bash
mkdir -p packages apps
```

### 2. Create a shared library

```bash
mkdir packages/greet && cd packages/greet
pnpm init
```

Edit `packages/greet/package.json`:
```json
{
  "name": "@mine/greet",
  "version": "0.1.0",
  "main": "index.js",
  "private": true
}
```

Create `packages/greet/index.js`:
```javascript
module.exports = {
  greet: (name) => `Hello, ${name}! From the monorepo.`,
};
```

### 3. Create an app that uses it

```bash
cd ~/mini-monorepo
mkdir apps/hello && cd apps/hello
pnpm init
```

Edit `apps/hello/package.json`:
```json
{
  "name": "hello-app",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "@mine/greet": "workspace:*"
  }
}
```

Notice `workspace:*` — tells pnpm to use the local package, not go to npm.

Create `apps/hello/index.js`:
```javascript
const { greet } = require('@mine/greet');
console.log(greet('Monorepo'));
```

### 4. Install and run

```bash
cd ~/mini-monorepo
pnpm install                              # links workspace packages
pnpm --filter hello-app start             # runs the app
```

You should see: `Hello, Monorepo! From the monorepo.`

### 5. Make a cross-package change

Edit `packages/greet/index.js` to add exclamation points:
```javascript
module.exports = {
  greet: (name) => `Hello, ${name}!!! From the monorepo!!!`,
};
```

Re-run:
```bash
pnpm --filter hello-app start
```

No rebuild. No republish. The change flows through because the app consumes the package directly from the workspace.

### 6. Capture the structure

```bash
cd ~/mini-monorepo
find . -type f -not -path './*/node_modules/*' -not -path './node_modules/*' -not -path './.git/*' | sort > ~/mini-monorepo-tree.txt
```

Save `mini-monorepo-tree.txt` to your submission.

## Submission

- `submission/inspection.md` — the 5 questions about the open-source monorepo
- `submission/mini-monorepo-tree.txt` — the file listing of your created monorepo
- `submission/app-output.md` — paste of the `pnpm --filter hello-app start` output, before AND after the exclamation point change
- `submission/reflection.md`:
  - When would you choose polyrepo over monorepo for *your* work?
  - What does `workspace:*` mean in a `package.json` dependency?
  - If you `git worktree add` a monorepo, how big is the new worktree's `node_modules/`? (pnpm's answer is different from npm's.)
  - One failure mode of a monorepo at scale that you wouldn't hit with polyrepo

## Pass criteria

- [ ] Successfully inspected a real open-source monorepo — answered all 5 questions with evidence
- [ ] Mini monorepo scaffolded and `pnpm --filter hello-app start` runs
- [ ] Cross-package change (Part B step 5) actually flowed through without a rebuild/republish
- [ ] Reflection answers are specific (not "polyrepo is simpler" — WHEN is it the right call?)

## You're done with foundations

After this, you're ready for the AI curriculum. Start Section 1 (Mental Models + When NOT to Use Agents).

## Common snags

- **`pnpm: command not found`** → `npm install -g pnpm` failed silently. Try `corepack enable && corepack prepare pnpm@latest --activate`.
- **`Cannot find module '@mine/greet'`** → you forgot to run `pnpm install` from the root after creating the workspace. Workspace links are created by install.
- **`pnpm init` asks many questions** → accept defaults. You'll edit the file anyway.
- **Cloning the open-source monorepo takes forever** → use `--depth 1` to only grab the latest commit. Much faster.
