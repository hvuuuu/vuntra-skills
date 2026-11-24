# Vuntra's skills

Agent skills for coding with AI, following the open [Agent Skills](https://agentskills.io) format (`SKILL.md`).

## Skills

| Skill | What it does ||---|---|
| [`project-foundation`](skills/project-foundation/SKILL.md) | Sets up or audits a project's foundation in any language: codebase structure (architecture, folders, boundaries, types) and static analysis (formatter, linter, types, tests, CI, agent hooks), while teaching the judgment behind each choice. |

## Install

```bash
# skills CLI (skills.sh)
npx skills add https://github.com/hvuuuu/vuntra-skills

# GitHub CLI
gh skill install https://github.com/hvuuuu/vuntra-skills project-foundation --agent claude-code
```

Or copy `skills/project-foundation/` into `~/.claude/skills/` (Claude Code) or your agent's skills folder.

## Why

Inspired by Lauren Tan's talk *All Code Is Slop (Except Mine)*: agents copy whatever the codebase already does, so the cheapest place to stop bad code is the codebase itself and the static checks around it, not longer rules files.

## License

MIT
