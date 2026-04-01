# Micepad Skills

Agent skills for the [Micepad](https://micepad.co) event management platform. Works with [Claude Code](https://claude.ai/code) and [30+ other AI coding tools](https://agentskills.io).

After installation, use `/micepad` in your AI coding tool to manage events, participants, check-ins, campaigns, and more — all from your editor.

## Install

**Prerequisites:** [Micepad CLI](https://github.com/micepadteam/micepad-cli) installed and authenticated.

```bash
# Install the CLI
curl -fsSL https://github.com/micepadteam/micepad-cli/releases/latest/download/install.sh | bash
micepad login

# Install the skill
npx skills add micepadteam/skills -g
```

## Available Skills

This repo contains two skills:

| Skill | Who it's for | Invoke with |
|-------|-------------|-------------|
| **micepad** | Event managers | `/micepad` |
| **micepad-admin** | Super admins only | `/micepad-admin` |

### `/micepad` — Event Management

Full CLI coverage for the complete event lifecycle — from creating the event to post-conference cleanup.

| Area | Examples |
|------|----------|
| **Events** | List events, set active event, view stats |
| **Participants** | List, search, filter, add, check in/out, import, export |
| **Check-ins** | Live stats, recent activity, velocity tracking |
| **Campaigns** | Create and send email/WhatsApp campaigns |
| **Groups** | Manage groups, view RSVP breakdowns |
| **Forms** | Build registration forms, manage fields |
| **Badges** | Create badge templates, add fields |
| **Sessions** | Manage event schedule and tracks |
| **Templates** | Browse email and message templates |

### `/micepad-admin` — Platform Administration

Platform-wide operations for Micepad super admins. Requires super admin access — regular event managers should use `/micepad` instead.

| Area | Examples |
|------|----------|
| **Dashboard** | Platform-wide stats (users, DAU, MAU, events, revenue) |
| **Accounts** | List and search all accounts |
| **Users** | Manage users, audit login sessions, force logout |
| **Gatherings** | Browse all events across accounts |
| **Subscriptions** | View active subscriptions |
| **Email Health** | Check deliverability, verify domain setup (SPF, DKIM, DMARC) |

## Selective Installation

Install only the skill you need:

```bash
# Event management skill only
npx skills add micepadteam/skills -g -s micepad

# Admin skill only (super admins)
npx skills add micepadteam/skills -g -s micepad-admin

# Both skills
npx skills add micepadteam/skills -g
```

## Manual Installation

```bash
git clone https://github.com/micepadteam/skills ~/.micepad-skills
mkdir -p ~/.claude/skills
ln -sfn ~/.micepad-skills/skills/micepad ~/.claude/skills/micepad
ln -sfn ~/.micepad-skills/skills/micepad-admin ~/.claude/skills/micepad-admin  # optional
```

## Update

```bash
npx skills update
```

Or update via the CLI:

```bash
micepad update  # updates both CLI binary and skills
```

## Skills Standard

This repo follows the [Agent Skills](https://agentskills.io) open standard, making it compatible with Claude Code, Cursor, VS Code (Copilot), Gemini CLI, and many more.

## License

MIT
