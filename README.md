# Micepad Skills

Agent skills for the [Micepad](https://micepad.co) event management platform. Works with [Claude Code](https://claude.ai/code) and [30+ other AI coding tools](https://agentskills.io).

After installation, use `/micepad` in your AI coding tool to manage events, participants, check-ins, campaigns, and more — all from your editor.

## Install

**Prerequisites:** [Micepad CLI](https://github.com/micepadteam/micepad-cli) installed and authenticated.

```bash
# Install the CLI
curl -fsSL https://raw.githubusercontent.com/micepadteam/micepad-cli/master/scripts/install.sh | bash
micepad login

# Install the skill
npx skills add micepadteam/skills
```

Or install globally:

```bash
npx skills add micepadteam/skills -g
```

### Manual Installation

```bash
git clone https://github.com/micepadteam/skills ~/.micepad-skills
mkdir -p ~/.claude/skills
ln -sfn ~/.micepad-skills/skills/micepad ~/.claude/skills/micepad
```

For per-project installation, symlink into `.claude/skills/` in your project directory instead.

## Usage

Once installed, the skill activates automatically when you mention Micepad, events, participants, check-ins, or campaigns. You can also invoke it directly:

```
/micepad events list
/micepad pax list --status confirmed
/micepad checkins stats
```

### What You Can Do

| Area | Examples |
|------|----------|
| **Events** | List events, set active event, view stats |
| **Participants** | List, search, filter, add, check in/out, import, export |
| **Check-ins** | Live stats, recent activity, velocity tracking |
| **Campaigns** | Create, send, monitor delivery stats |
| **Groups** | List groups, view RSVP breakdowns |
| **Templates** | Browse email and message templates |
| **Admin** | Platform dashboard, account management (super admin) |

## Skills Standard

This repo follows the [Agent Skills](https://agentskills.io) open standard, making it compatible with Claude Code, Cursor, VS Code (Copilot), Gemini CLI, and many more.

## License

MIT
