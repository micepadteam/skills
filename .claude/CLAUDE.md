# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This repo distributes **Micepad agent skills** for Claude Code and other AI coding tools via the [Agent Skills](https://agentskills.io) open standard. It contains no application code — only skill definitions (Markdown), plugin metadata (JSON), and documentation.

## Repo Structure

```
skills/
  micepad/SKILL.md          # Main skill: event management via Micepad CLI (user-facing)
  micepad-admin/SKILL.md    # Admin skill: platform administration (super admin only)
.claude-plugin/
  plugin.json               # Plugin manifest (name, version, description)
  marketplace.json          # Marketplace categories
install.md                  # Agent-executable installation guide
AGENTS.md                   # Repo maintenance guide
```

## Editing Skills

Each skill is a single `SKILL.md` file with:
- **YAML frontmatter**: `name`, `description`, `triggers`, `metadata` (version), `invocable`, `argument-hint`
- **Markdown body**: Full instructions loaded when the skill activates

### Key Constraints

- Keep skill body **under 5000 tokens** for fast loading
- `triggers` list controls when the skill auto-activates — update when adding new command areas
- `micepad` skill (v0.3.0) is public; `micepad-admin` skill (v0.1.0) is proprietary/private
- Test locally before publishing: `npx skills add . -a claude-code`

### Content Conventions in SKILL.md

- Command reference uses pipe-delimited tables (`| Command | What it does |`)
- Multi-step workflows use numbered headings with code blocks
- The micepad skill follows a "6 Acts" lifecycle narrative for end-to-end event setup
- Agent invariants (rules the AI must follow) go at the top of the skill body
- CLI conventions for list commands (filter/search/pagination flags) are documented once and shared across both skills

## No Build/Test/Lint

This is a content-only repo. There are no build steps, test suites, or linters. Validation is manual: read the skill, test with `npx skills add . -a claude-code`, verify in Claude Code via `/micepad`.
