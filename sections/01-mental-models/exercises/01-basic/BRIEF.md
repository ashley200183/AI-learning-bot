# Exercise 1.1 — Observe Skill Triggering + Anti-patterns

**Time estimate:** 45–75 minutes
**Dependencies:** Claude Code CLI; access to plugin marketplace

## Objective

Experience the "does it trigger?" problem firsthand. Then articulate three tasks where reaching for an agent would be the wrong move.

## Setup — install the example skills

Run in Claude Code:

```
/plugin marketplace add anthropics/skills
/plugin install example-skills@anthropic-agent-skills
```

**Verify:** run `/plugin list` — you should see `example-skills` listed. If it's missing, the marketplace add likely failed; re-run and check for error output. If the marketplace itself isn't reachable, confirm you're online and your Claude Code version is current (`claude --version`).

## Requirements

1. In a fresh prompt, say: *"Create a PDF summary of this project."* Do NOT name the PDF skill.
2. Observe: did Claude name the PDF skill? Did it auto-invoke, or produce generic content?
3. Now say: *"Use the PDF skill to create a PDF summary."* Observe the difference.
4. Write `submission/observations.md`:
   - Did step 2 auto-trigger? Yes/no/partial — with transcript evidence.
   - What specifically changed in step 4?
   - One-sentence hypothesis: *what makes a skill description trigger reliably?*
   - Paste the key transcript excerpts (don't need the whole thing).
5. Write `submission/anti-patterns.md`: three real tasks from your work or life where building an agent would be the wrong move. For each:
   - The task
   - The sane alternative (ask Claude directly? do it manually? something else?)
   - Why the agent form would be worse

## Submission

- `submission/observations.md`
- `submission/anti-patterns.md`

## Rubric

Pass: **total ≥ 15 / 20 AND all `[GATE]` criteria ≥ 3/5.**

| Criterion | Gate | Score | Notes |
|---|---|---|---|
| **Evidence** — both steps captured with transcripts | `[GATE]` | /5 | Gate: no submission if transcripts are missing |
| **Observation accuracy** — correctly identified auto-trigger behavior | — | /5 | Specific, not vague |
| **Hypothesis** — informed by the observed difference | — | /5 | Cites what changed between steps 2 and 4 |
| **Anti-patterns** — three realistic examples, each defended | `[GATE]` | /5 | Gate: anti-patterns can't be hypothetical "someone might..." — must be from actual tasks you face |

## What "good" looks like

Your observation reflection cites the *exact* sentence where Claude's behavior shifted. Your hypothesis is something like: *"Descriptions trigger reliably when they include action verbs the user is likely to type (create, generate, make) plus the artifact noun (PDF)."* Your anti-patterns include something like: *"Writing one-line shell scripts I'll run once — an agent is 10x the overhead for no leverage."*

## Integration with prior sections

None — first exercise.
