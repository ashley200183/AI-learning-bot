# Starter Project Template

Copy this skeleton into your cumulative exercise submission when the BRIEF says so:

```bash
cp -r _starter-project/ sections/NN-<slug>/exercises/02-cumulative/submission/project/
```

This gives you the standard `.claude/` layout used across Claude Code projects. Populate the empty directories with your prior-section artifacts as the BRIEF directs.

## Structure

```
.claude/
  skills/            # put your skills here (one folder per skill, each containing SKILL.md)
  commands/          # put custom commands here (one .md file per command)
  agents/            # put subagent definitions here (one .md file per agent)
  settings.json      # hooks config (optional, empty object by default)
CLAUDE.md            # project-level standing orders (starter version provided)
notes/
  .gitkeep           # handoffs, sprint context, etc. live here
```

## Use

1. Copy the whole folder into your submission.
2. Do NOT edit files in `_starter-project/` itself — always copy, then edit the copy.
3. Populate per the BRIEF's instructions.
4. If the BRIEF says "use your S2.1 commit-style skill," copy that skill into `.claude/skills/commit-style/`.

## Non-rules

- You can add folders (e.g., `.claude/logs/`) as needed.
- You can edit the starter `CLAUDE.md` freely — it's the minimum, not a maximum.
- If a cumulative exercise doesn't need this template (it's noted in the BRIEF), don't copy it.
