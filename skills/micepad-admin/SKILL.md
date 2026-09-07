---
name: micepad-admin
description: >
  Micepad platform administration for super admins. Monitor platform health,
  accounts, users, events, subscriptions, email and WhatsApp delivery,
  suppressions, webhooks, and release settings.
  Private skill — requires super admin access.
license: proprietary
compatibility: Requires the Micepad CLI installed and authenticated as a super admin.
metadata:
  author: Micepad Team
  version: 0.2.0
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
  - micepad wacheck
  - micepad webhooks
  - micepad suppressions
  - micepad print-helper
---

# Micepad Admin

Use this private skill for platform-wide super admin work. Use `/micepad` for regular event operations.

## Discovery and safety

Checked against CLI **0.4.9** and the **dev** server tree. The server supplies commands; prod/alpha may differ.

```bash
micepad version
micepad whoami
micepad admin help
micepad admin users help disable
micepad admin emailcheck help account
```

1. Verify environment and super admin access before acting. Do not switch environments without the user's request.
2. Never guess commands, required arguments or flags. Use `micepad tree` and nested help. Preserve underscores or hyphens from help.
3. Read before writing. Confirm the exact account/user/event and scope.
4. Get approval before forced logouts, disabling users, role changes, suppression changes, test sends, resuming campaigns, or changing platform settings.
5. Never expose credentials, session tokens, raw webhook payloads, or private user data. Redact diagnostics.
6. `--json` is advertised but server behavior varies. Validate the actual response before parsing; do not assume a CLI-wide failure or universal support.

```text
Verify environment + admin -> inspect target -> confirm change -> verify result
```

## Command map

Use explicit `list` commands rather than relying on group defaults. Commands below are discovery paths; inspect their help for arguments before execution.

| Area | Read commands | Commands requiring care |
|------|---------------|-------------------------|
| Platform | `admin dashboard`, `admin funnels` | — |
| Accounts | `admin accounts list` | `admin accounts add_owner`, `admin accounts add_user` |
| Users | `admin users list`, `admin users sessions`, `admin users app_version` | `admin users delete_session`, `disable`, `enable`, `set_app_version` |
| Events | `admin gatherings list` | — |
| Subscriptions | `admin subscriptions list` | — |
| Email | `admin emailcheck app`, `accounts`, `list`, `templates`, `status` | `admin emailcheck account`, `send`, `campaign`, `resume` |
| WhatsApp | `admin wacheck app`, `list`, `status` | `admin wacheck account`, `send`, `campaign` |
| Suppressions | `admin suppressions list`, `global_list`, `check` | `admin suppressions add`, `remove`, `global_add`, `global_remove`, `sync` |
| Incoming webhooks | `admin webhooks list`, `show`, `stats` | Treat details as sensitive |
| AI settings | `admin ai-settings show` | `admin ai-settings set` |
| Print Helper releases | `admin print-helper show` | `admin print-helper set` |

Shorthand entries stay under the table's command area, e.g. `admin users enable`, not `admin enable`.

## Lists and filters

Check each command's help: flags and defaults vary. Users, accounts and gatherings support `--search`; gatherings also supports `--status`. Common flags:

| Flag | Purpose |
|------|---------|
| `--filter` | Supported Ransack predicates, e.g. `name_cont=acme` |
| `--limit` | Page size; users default to 20, email account audits to 50 |
| `--page` | Page number, generally 1 by default |
| `--json` | Request JSON; verify actual output |

Combine supported filters with commas, e.g. `name_cont=acme,created_at_gteq=2026-01-01`. Never assume every model accepts every predicate.

## Read-only platform check

```bash
micepad admin dashboard
micepad admin funnels
micepad admin gatherings list --status published --limit 20
micepad admin subscriptions list
micepad admin emailcheck app
micepad admin emailcheck accounts --limit 50
micepad admin webhooks stats
```

Summarize findings before proposing changes. Paginate when a complete audit is needed.

## Locate a user or account

```bash
micepad admin users list --search "user@example.com"
micepad admin accounts list --search "acme"
micepad admin users sessions USER_ID
micepad admin users sessions USER_ID --current
```

`--current` selects sessions active in the last 24 hours. After explicit approval, force logout with:

```bash
micepad admin users delete_session USER_ID SESSION_ID
# Or, only with approval for all devices:
micepad admin users delete_session USER_ID --all
```

Re-list sessions to verify. Do not echo IP/device details unless needed for the user's audit.

## Email deliverability

Start with read-only checks:

```bash
micepad admin emailcheck app
micepad admin emailcheck accounts
```

**`emailcheck account ACCOUNT_ID` sends a test email** to the admin login address in addition to checking SES/DNS/reputation. It is not a read-only domain check. Ask first, then:

```bash
micepad admin emailcheck account ACCOUNT_ID
micepad admin emailcheck status TRACKING_ID --watch
```

Use the returned tracking ID. `--watch` polls for a terminal result with a bounded timeout; a queued message is not proof of delivery. Inspect `help send`, `help campaign`, or `help resume` before those operations. Resuming a paused campaign may send real recipient mail.

WhatsApp `account`, `send`, and `campaign` also send messages. Confirm the recipient and scope. Never remove account/global suppressions simply to make a test succeed.
