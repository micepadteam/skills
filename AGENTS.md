# Skills Repo — Maintenance Guide

This repository distributes Micepad agent skills for Claude Code and other AI coding tools.

## Structure

```
skills/
  micepad/
    SKILL.md          # The skill definition (Agent Skills standard)
.claude-plugin/
  plugin.json         # Plugin manifest
  marketplace.json    # Marketplace metadata
install.md            # Agent-executable installation guide
README.md             # User-facing documentation
```

## Editing Skills

Edit skill definitions directly in `skills/micepad/SKILL.md`. The file follows the [Agent Skills](https://agentskills.io) open standard.

### SKILL.md Format

- **YAML frontmatter**: name, description, triggers, metadata
- **Markdown body**: Instructions loaded when the skill activates

### Guidelines

- Keep the skill body under 5000 tokens for fast loading
- Use the quick reference table for common commands
- Add workflow examples for multi-step operations
- Update triggers when new command areas are added
- Test with `npx skills add . -a claude-code` locally before publishing
