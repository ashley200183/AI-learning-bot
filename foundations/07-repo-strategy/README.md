# F7 — Repo Strategy: Mono vs Poly

**Time:** 30–45 min (seen monorepos) · 60–75 min (first time)
**Prerequisites:** F3, F4, F5 complete

## Why this matters

Sooner or later you'll have to choose — or inherit — a way of organizing code across projects. Do you put everything in one repo (**monorepo**) or give each project its own repo (**polyrepo**, sometimes called "multi-repo" or "flat")? The choice affects how you branch, how you deploy, how worktrees behave, and how AI agents work across your code.

## The two shapes

### Polyrepo (flat)

```
github.com/me/
  my-website/              ← own repo
  my-api/                  ← own repo
  my-cli-tool/             ← own repo
  shared-ui-library/       ← own repo (published as a package)
```

Each project is independent. If `my-website` uses `shared-ui-library`, it consumes it as a published npm package.

### Monorepo

```
github.com/me/platform/    ← one repo
  apps/
    website/
    api/
    cli-tool/
  packages/
    shared-ui/             ← consumed directly by apps/* via workspaces
    shared-types/
    shared-utils/
  package.json             ← root, declares workspaces
```

One repo, multiple packages. Changes across packages happen in a single commit.

## Trade-offs

| | Monorepo | Polyrepo |
|---|---|---|
| **Cross-package refactoring** | Easy — one PR changes all consumers | Hard — need coordinated releases |
| **Dependency management** | Shared versions enforced at root | Each repo pins its own versions (can drift) |
| **CI/CD complexity** | Harder — need to know what changed and build only that | Simpler — each repo has its own pipeline |
| **Tooling overhead** | Workspace tools required (pnpm workspaces, turborepo, nx) | None needed |
| **Onboarding** | "Clone one thing and you have it all" | "Here are the 12 repos you'll need" |
| **Merge conflicts** | More in a shared area | Scoped per repo |
| **Permissions / access** | All-or-nothing usually | Per-repo control |
| **Worktrees** | Each worktree is the *whole* monorepo | Each worktree is just that one project |
| **Scale** | Google, Meta do it with 10,000+ developers | Most open source |

### When monorepo wins

- You have multiple apps sharing code that changes often
- You want atomic cross-package commits ("update the API and the client in one PR")
- You're a small team and want to reduce coordination cost
- You care about version consistency across apps

### When polyrepo wins

- Projects are independent and released on different schedules
- Different teams own different projects
- You want each project to be forkable/shareable independently
- You don't want to learn workspace tools

### The honest answer

**Start polyrepo until you feel the pain.** The pain point is usually: "I changed the shared library and now I need to bump versions in 4 downstream repos and wait for each to pick up the change." When that becomes frequent, consider monorepo.

## Workspace tools for monorepos

If you go monorepo, you'll want a workspace tool that handles installing, building, and running commands across packages.

| Tool | Language | Strength |
|---|---|---|
| **pnpm workspaces** | JS/TS | Minimal, fast, built into pnpm |
| **npm workspaces** | JS/TS | Built into npm; fewer features than pnpm |
| **yarn workspaces** | JS/TS | Mature; yarn v1 still common |
| **Turborepo** | JS/TS | Adds intelligent task caching; pairs with pnpm |
| **Nx** | JS/TS + more | Full-featured; heavier; good for large monorepos |
| **Lerna** | JS/TS | Older; largely superseded by the above |
| **Rush** | JS/TS (Microsoft) | Enterprise-scale monorepo management |
| **Bazel** | Any | Google-scale; steep learning curve |
| **Cargo workspaces** | Rust | Built into cargo |
| **Go modules + monorepo** | Go | Works natively; `go.work` files |

For a first monorepo: **pnpm workspaces + Turborepo** is a sensible default for JS/TS. No Bazel until you're at serious scale.

## Directory conventions

### Typical polyrepo layout

```
my-api/
  src/               ← source code
  tests/             ← tests
  docs/              ← documentation
  scripts/           ← build/deploy scripts
  .github/           ← CI/CD workflows
  package.json
  README.md
  LICENSE
  .gitignore
```

### Typical monorepo layout (pnpm workspaces)

```
my-platform/
  apps/              ← deployable applications
    web/
    api/
    admin-dashboard/
  packages/          ← shared internal libraries (not published)
    ui/
    types/
    utils/
  tools/             ← repo-level scripts, codemods
  .github/
  package.json       ← root; declares workspaces
  pnpm-workspace.yaml
  turbo.json         ← (if using Turborepo)
  README.md
```

### Conventions you'll see

- **`src/`** — source code (as opposed to build output)
- **`lib/`** — compiled/built output, or sometimes the main library code in older projects
- **`dist/`** or **`build/`** — bundled output ready to deploy (always in `.gitignore`)
- **`public/`** or **`static/`** — web assets served as-is
- **`tests/`** or **`__tests__/`** or colocated `*.test.ts` — tests
- **`docs/`** — long-form docs (README is short-form)
- **`scripts/`** — maintenance scripts not part of the app
- **`.github/workflows/`** — GitHub Actions CI/CD definitions
- **`.vscode/`** — VS Code editor config (usually gitignored or committed selectively)

## Worktrees in monorepos — the subtle bit

In F5 you learned `git worktree add`. One thing to know for monorepos:

**A worktree is always the whole repo.** You can't worktree just one package.

That means:
- `git worktree add ../monorepo-feature-x -b feature-x` gives you the **entire** monorepo in that directory — all apps, all packages.
- If your monorepo is big (gigabytes of `node_modules/` after install), each worktree pays that cost separately.
- Some workspace tools (pnpm, with its content-addressable store) share node_modules across worktrees — much cheaper. npm and yarn v1 don't.

**Implication for AI agents (AI curriculum Section 13):** running 5 parallel worktree agents on a monorepo means 5x the disk for dependencies. Budget accordingly, and strongly prefer pnpm if you're doing this at any scale.

## Security: shared vs isolated secrets

- **Polyrepo:** each repo has its own `.env.example`. Clean isolation. Low risk that a dev who works on the API sees the website's secrets.
- **Monorepo:** typically one root `.env` with *all* secrets. Everyone who clones the repo can see all of them (when they set up `.env` locally). For small teams fine; for larger teams consider per-package env files or a secrets manager.

## Exercise

See `exercises/01-basic/BRIEF.md`. You'll inspect a real monorepo, reason about package boundaries, and set up a minimal one locally.
