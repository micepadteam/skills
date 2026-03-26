---
name: micepad
description: >
  Interact with the Micepad event management platform via the Micepad CLI.
  Use for ANY Micepad question or action: managing events, participants,
  check-ins, campaigns, groups, templates, and admin operations.
  Full CLI coverage for event operations, participant management,
  bulk imports/exports, campaign delivery, and live check-in monitoring.
license: MIT
compatibility: Requires the Micepad CLI binary (`micepad`) installed and authenticated.
metadata:
  author: Micepad Team
  version: 0.1.0
  homepage: https://github.com/micepadteam/skills
invocable: true
argument-hint: "[action] [args...]"
triggers:
  - micepad
  - event management
  - participant
  - attendee
  - check-in
  - checkin
  - campaign
  - pax
  - micepad event
  - micepad participant
  - micepad checkin
  - micepad campaign
  - micepad import
  - micepad export
  - studio.micepad.co
  - launchpad.micepad.co
---

# Micepad CLI Skill

You are an agent that helps users interact with the **Micepad** event management platform through the `micepad` CLI.

## Agent Invariants

You MUST follow these rules at all times:

1. **Never fabricate CLI commands.** Only use commands documented here or discovered via `micepad terminal tree` / `micepad help`.
2. **Always confirm destructive actions** (sending campaigns, bulk imports with `--yes`, cancelling campaigns) before executing.
3. **Prefer reading before writing.** List/show before create/update/delete.
4. **Respect the active event context.** Many commands operate on the currently selected event. Use `micepad whoami` to verify context before taking action.
5. **Handle authentication gracefully.** If a command fails with auth errors, suggest `micepad login`.
6. **Never expose or log credentials, tokens, or session data.**

## CLI Introspection

When you encounter an unfamiliar command or need to verify syntax:

```bash
micepad terminal tree    # Full command tree with all subcommands
micepad help             # Top-level help
micepad events help      # Subcommand-specific help
micepad pax help         # Participant commands help
```

Always introspect before guessing. The CLI is server-driven — new commands may exist that aren't documented here.

## Quick Reference

| Command | What it does |
|---------|-------------|
| `micepad login` | Authenticate via browser |
| `micepad logout` | Clear session |
| `micepad whoami` | Show current user, account, and active event |
| `micepad events list` | List all events |
| `micepad events use SLUG` | Set active event context |
| `micepad events current` | Show active event details |
| `micepad events stats` | Event dashboard statistics |
| `micepad pax list` | List participants (supports filters) |
| `micepad pax show ID` | Show participant details |
| `micepad pax add` | Add a participant (interactive) |
| `micepad pax checkin ID` | Check in a participant |
| `micepad pax checkout ID` | Check out a participant |
| `micepad pax count` | Count participants by status |
| `micepad pax export` | Export participants to CSV |
| `micepad pax import FILE` | Import from CSV/Excel |
| `micepad checkins stats` | Check-in statistics with velocity |
| `micepad checkins stats --watch` | Live-refresh stats (2s interval) |
| `micepad checkins recent` | Recent check-in activity |
| `micepad campaigns list` | List campaigns |
| `micepad campaigns create` | Create a campaign |
| `micepad campaigns show ID` | Campaign details |
| `micepad campaigns send ID` | Send a campaign |
| `micepad campaigns cancel ID` | Cancel scheduled campaign |
| `micepad campaigns stats ID` | Delivery statistics |
| `micepad templates list` | List templates |
| `micepad groups list` | List groups |
| `micepad groups show NAME` | Group details with RSVP breakdown |
| `micepad admin dashboard` | Platform stats (super admin) |
| `micepad admin accounts` | List accounts (super admin) |
| `micepad admin users` | List users (super admin) |
| `micepad admin gatherings` | List gatherings (super admin) |
| `micepad admin subscriptions` | List subscriptions (super admin) |
| `micepad terminal tree` | Show full command tree |

## Participant Identifiers

Commands that accept a participant ID support multiple formats:

- **Prefix ID**: `pax_abc123`
- **Email**: `john@example.com`
- **Code**: the participant's registration code
- **QR Code**: the participant's QR code value

```bash
micepad pax show pax_abc123
micepad pax show john@example.com
micepad pax checkin pax_abc123
```

## Filtering Participants

```bash
micepad pax list --status confirmed
micepad pax list --checkin checked_in
micepad pax list --checkin not_checked_in
micepad pax list --group "VIP"
micepad pax list --search "john"
micepad pax list --status confirmed --checkin not_checked_in --limit 100
```

## Importing Participants

Import is a multi-step process. Follow this order:

```bash
# 1. Check the storage path (files must be placed here first)
micepad pax import --storage-path

# 2. Download a template (optional)
micepad pax import --template
micepad pax import --template --format xlsx

# 3. Copy the file to storage
cp attendees.csv $(micepad pax import --storage-path)/

# 4. Run import (interactive by default — prompts for group, mappings)
micepad pax import attendees.csv

# 5. Or run with options
micepad pax import attendees.csv --group "VIP" --action add --yes
micepad pax import attendees.csv --dry-run
micepad pax import attendees.csv --errors-out errors.csv
```

**Important:** The CLI sandboxes file access. Files must be in the storage directory before import. Always copy files there first.

## Campaign Workflows

```bash
# List email campaigns
micepad campaigns list --type email

# Create from template
micepad campaigns create --type email --name "Welcome" --template tpl_xxxxx

# Send (ALWAYS confirm with user first)
micepad campaigns send cmp_xxxxx

# Monitor delivery in real-time
micepad campaigns stats cmp_xxxxx --watch
```

## Common Workflows

### Event Day Operations

```bash
# Set the event context
micepad events use my-conference-2025

# Monitor check-ins live
micepad checkins stats --watch

# Quick check-in by scanning/email
micepad pax checkin john@example.com

# See recent activity
micepad checkins recent

# Get a headcount
micepad pax count
```

### Pre-Event Preparation

```bash
# Review event details
micepad events current
micepad events stats

# Import attendee list
cp registrations.csv $(micepad pax import --storage-path)/
micepad pax import registrations.csv --dry-run
micepad pax import registrations.csv --group "General" --action add

# Send confirmation emails
micepad campaigns create --type email --name "Confirmation" --template tpl_xxxxx
micepad campaigns send cmp_xxxxx
```

### Reporting

```bash
# Export all participants
micepad pax export

# Check-in stats
micepad checkins stats

# Campaign delivery stats
micepad campaigns stats cmp_xxxxx

# Group breakdown
micepad groups show "VIP"
```

### Admin Operations (super admin only)

```bash
micepad admin dashboard
micepad admin accounts --search "acme"
micepad admin gatherings --status published --limit 50
```

## Configuration

The CLI connects to `wss://studio.micepad.co/terminal` by default. Override with:

```bash
export MICEPAD_URL="wss://studio.micepad.co/terminal"  # Production
export MICEPAD_URL="ws://localhost:3000/terminal"        # Local dev
```

## Error Handling

- **Authentication errors**: Run `micepad login` to re-authenticate
- **No active event**: Run `micepad events use SLUG` to set context
- **File not found on import**: Ensure file is copied to the storage directory first
- **Permission denied**: Some commands (admin) require super admin access
