# Exercise 16.1 — Diagnose Three Planted Failures

**Time estimate:** 1.5 hours
**Dependencies:** Claude Code CLI; your own skills/commands from earlier sections

## Objective

Deliberately cause three failures — one per class (trigger, context, instruction) — and diagnose each using the systematic approach. (Capability failures are harder to plant meaningfully; we'll skip that class here.)

## Requirements

### Failure A — Trigger failure

1. Take one of your existing skills (S2.1 or S2.2). Copy it to `submission/failure-a/original-skill/`.
2. Weaken its description: remove all trigger phrases, leaving only a bland statement like *"For handling git operations."*
3. In a fresh session, invoke a prompt that *should* have triggered the original. Observe it doesn't.
4. Diagnose: write `submission/failure-a/diagnosis.md` answering:
   - What class of failure?
   - Specific evidence — what in the description caused the miss?
   - Fix: exact description change that restores triggering.
5. Apply the fix. Confirm it works. Document in `submission/failure-a/fix-verification.md`.

### Failure B — Context failure

1. Start a session with a clear early instruction (e.g., *"Use Prisma for all DB queries in this session; never raw SQL."*).
2. Work for 30+ minutes on any task that involves DB decisions. Read several large files. Do at least 10 tool calls.
3. At some point, prompt Claude to "quickly write a query for X" — it will probably write raw SQL if the window is full enough.
4. If it doesn't fail naturally, prompt increasingly far from the original instruction until it does.
5. Save the full transcript to `submission/failure-b/transcript.md` with the failure point flagged.
6. Diagnose: `submission/failure-b/diagnosis.md`:
   - What class of failure?
   - What was the token count at failure (use `/cost`)?
   - What mechanism failed — was it out of window, or drowned by tool results?
   - Fix strategy — which of S6's three strategies would have prevented this?

### Failure C — Instruction failure

1. Write a CLAUDE.md rule that *sounds* clear but is actually ambiguous. Example: *"Always validate inputs."* (Validate how? Zod? Manual checks? At the route handler? In the business logic?)
2. In a session with that CLAUDE.md, ask Claude to build something with inputs. Observe what it does.
3. Evidence: `submission/failure-c/evidence.md` — transcript showing Claude's interpretation wasn't what you wanted.
4. Diagnose: `submission/failure-c/diagnosis.md`:
   - What class of failure?
   - Quote the exact instruction that was ambiguous
   - Rewrite: unambiguous version
5. Apply the rewrite. Confirm behavior shifts. Document.

## Submission

- `submission/failure-a/` — skill-weakening + diagnosis + fix
- `submission/failure-b/` — context-failure transcript + diagnosis
- `submission/failure-c/` — ambiguous-instruction + rewrite
- `submission/reflection.md`:
  - Which class was easiest to diagnose? Hardest?
  - What tool (`/diff`, `/cost`, logs, etc.) was most useful?
  - A pattern you'll now watch for in your own work.

## Rubric

Pass: **total ≥ 22 / 30 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Failure A reproduced and diagnosed** | `[GATE]` | /5 | Gate: just a description rewrite without showing the failure = fail |
| **Failure B reproduced and diagnosed** | `[GATE]` | /5 | Gate: no real long session = synthetic = fail |
| **Failure C reproduced and diagnosed** | `[GATE]` | /5 | Gate: ambiguity must be specifically cited + rewrite shown |
| **Each class correctly identified** | — | /5 | Not conflated |
| **Fix verifications** — each fix re-tested | — | /5 | |
| **Reflection** — diagnostic tool ranking + watch-pattern | — | /5 | |

## What "good" looks like

Failure A: description said *"For git tasks"*. Prompt "push this up" missed. Fix: add *"Use whenever writing a commit message, running git commit, or preparing to push, even when the user says 'push', 'ship it', 'save this', or 'commit'."* Re-test: triggers. Failure B caught at 172K tokens, after 14 tool calls, Claude wrote raw SQL despite initial Prisma instruction. Failure C: *"Always validate"* became *"Validate at the route handler using Zod; reject with 400 if invalid; never validate silently downstream."*

## Integration with prior sections

Uses your own skills/CLAUDE.md as the material for failures. Draws on S2, S5, S6 depending on which failures you plant — not formally required.
