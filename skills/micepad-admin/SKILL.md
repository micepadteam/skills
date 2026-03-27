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

### User Management

| Command | What it does |
|---------|-------------|
| `micepad admin users` | List all users |
| `micepad admin users --search "john"` | Search users |
| `micepad admin users sessions USER_ID` | List login sessions (IP, device, location, timestamps) |
| `micepad admin users sessions USER_ID --current` | Show active sessions only |
| `micepad admin users sessions delete USER_ID SESSION_ID` | Force logout a specific session |
| `micepad admin users sessions delete USER_ID --all` | Force logout all sessions for a user |

### Gatherings (Events)

| Command | What it does |
|---------|-------------|
| `micepad admin gatherings` | List all gatherings across accounts |
| `micepad admin gatherings --status published` | Filter by status |
| `micepad admin gatherings --limit 50` | Limit results |

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
micepad admin users sessions usr_RErUY --current    # Active sessions only
micepad admin users sessions delete usr_RErUY sess_abc12  # Force logout one session
micepad admin users sessions delete usr_RErUY --all       # Force logout all sessions
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
