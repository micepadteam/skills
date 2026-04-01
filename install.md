# Install Micepad Skills

Follow these steps to install the Micepad CLI and skill.

## Prerequisites

- [ ] Install the Micepad CLI binary
- [ ] Authenticate with Micepad
- [ ] Install the skill

## Step 1: Install the Micepad CLI

```bash
curl -fsSL https://raw.githubusercontent.com/micepad/micepad-cli/master/scripts/install.sh | bash
```

Verify installation:

```bash
micepad --version
```

## Step 2: Authenticate

```bash
micepad login
```

This opens your browser for authorization. Once complete, verify:

```bash
micepad whoami
```

## Step 3: Install the Skill

```bash
npx skills add micepad/skills
```

Or install globally (available in all projects):

```bash
npx skills add micepad/skills -g
```

Verify:

```bash
npx skills list
```

## Step 4: Verify

Open Claude Code and try:

```
/micepad events list
```

You should see your events listed.
