---
name: micepad-admin
description: >
  Micepad platform administration for super admins. Monitor platform health,
  manage accounts, users, gatherings, subscriptions, and email deliverability.
  Private skill — requires super admin access to the Micepad platform.
license: proprietary
compatibility: Requires the Micepad CLI binary (`micepad`) installed and authenticated as a super admin.
metadata:
  author: Micepad Team
  version: 0.1.0
invocable: true
argument-hint: "[action] [args...]"
triggers:
  - micepad admin
  - micepad dashboard
  - micepad accounts
  - micepad users
  - micepad subscriptions
  - micepad gatherings
  - micepad emailcheck
  - platform stats
  - admin dashboard
  - email health
  - email deliverability
  - new signups
  - login sessions
  - audit log
  - ip address
---

# Micepad Admin Skill

You are an agent that helps **Micepad super admins** manage the platform. This skill covers platform-wide operations: monitoring health, managing accounts, users, gatherings, subscriptions, and email deliverability.

This is a **private skill** — it requires super admin access. Regular event managers should use the `/micepad` skill instead.

## CLI Conventions for List Commands

All admin `list` commands follow a consistent set of flags:

### Standard Flags (all admin list commands)

| Flag | Purpose | Default |
|------|---------|---------|
| `--filter=FILTER` | Ransack filter for advanced queries (e.g. `name_cont=acme,status_eq=active`) | — |
| `--limit=N` | Max rows to return | 20 |
| `--page=N` | Page number for pagination | 1 |
| `--json` | Output as JSON (currently broken — returns table format) | false |

### Selective Flags

| Flag | Available on | Purpose |
|------|-------------|---------|
| `--search=TEXT` | `admin users`, `admin accounts`, `admin gatherings` | Fuzzy search by name or email |
| `--status=STATUS` | `admin gatherings` | Filter by event status |

### Ransack Filter Syntax

The `--filter` flag uses Ransack predicates. Common patterns:
- `name_cont=acme` — name contains "acme"
- `email_cont=example.com` — email contains domain
- `status_eq=active` — exact match
- `created_at_gteq=2026-01-01` — created on or after date
- Combine with commas: `name_cont=acme,status_eq=active`

## Agent Invariants

1. **Never fabricate CLI commands.** Only use commands documented here or discovered via `micepad tree`.
2. **Verify super admin context.** Run `micepad whoami` before admin operations to confirm access level.
3. **Prefer reading before writing.** List/show before any modifications.
4. **Never expose credentials, tokens, or sensitive user data** in responses.

## Admin Commands

### Platform Dashboard

```bash
micepad admin dashboard    # Platform-wide statistics (users, DAU, MAU, events, revenue)
```

### Account Management

| Command | What it does |
|---------|-------------|
| `micepad admin accounts` | List all accounts |
| `micepad admin accounts --search "acme"` | Search accounts by name |
| `micepad admin accounts --filter "name_cont=acme"` | Ransack filter |
| `micepad admin accounts --limit 50 --page 2` | Pagination |

### User Management

| Command | What it does |
|---------|-------------|
| `micepad admin users` | List all users |
| `micepad admin users --search "john"` | Search users by name or email |
| `micepad admin users --filter "email_cont=example.com"` | Ransack filter |
| `micepad admin users --limit 50 --page 2` | Pagination |
| `micepad admin users sessions USER_ID` | List login sessions (IP, device, login time, last activity) |
| `micepad admin users sessions USER_ID --current` | Show active sessions only (last 24h) |
| `micepad admin users delete-session USER_ID SESSION_ID` | Force logout a specific session |
| `micepad admin users delete-session USER_ID --all` | Force logout all sessions for a user |

### Gatherings (Events)

| Command | What it does |
|---------|-------------|
| `micepad admin gatherings` | List all gatherings across accounts |
| `micepad admin gatherings --search "summit"` | Search by event name |
| `micepad admin gatherings --status published` | Filter by status |
| `micepad admin gatherings --filter "name_cont=summit"` | Ransack filter |
| `micepad admin gatherings --limit 50 --page 2` | Pagination |

### Subscriptions

| Command | What it does |
|---------|-------------|
| `micepad admin subscriptions` | List active subscriptions |

### Email Health

| Command | What it does |
|---------|-------------|
| `micepad admin emailcheck` | Check email deliverability for current account |
| `micepad admin emailcheck account` | Verify domain setup (SPF, DKIM, DMARC) |

## Common Workflows

### New Signup Monitoring

```bash
micepad admin dashboard                    # Check overall stats
micepad admin users --search "new-user"    # Find specific user
micepad admin accounts --search "acme"     # Find their account
```

### Email Deliverability Check

```bash
micepad admin emailcheck                   # Overall email health
micepad admin emailcheck account           # Domain verification details
```

### User Session Audit

```bash
micepad admin users sessions usr_RErUY              # All login sessions
micepad admin users sessions usr_RErUY --current    # Active sessions only (last 24h)
micepad admin users delete-session usr_RErUY ses_abc12  # Force logout one session
micepad admin users delete-session usr_RErUY --all      # Force logout all sessions
```

Output includes: session ID, IP address, device/user agent, location, login time, last activity.

### Platform Health Check

```bash
micepad admin dashboard                    # DAU, MAU, total users
micepad admin gatherings --status published --limit 20  # Active events
micepad admin subscriptions                # Revenue/subscription status
```

## Known Issues

- **`--json` flag is broken.** Commands like `micepad admin users --json` still return table format instead of JSON. This affects commands across the CLI, not just admin. Do not rely on `--json` for machine-readable output until this is fixed server-side.
