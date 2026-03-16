# Claude Code Chief of Staff

> Personal automation layer for Claude Code — email triage, calendar management, task orchestration.

Inspired by [Jim Prosser's claude-code-cos](https://github.com/jimprosser/claude-code-cos) architecture.

## Architecture

Four-layer automation system:

| Layer | Description | Mode |
|-------|-------------|------|
| 1. Calendar Transit | Calculate drive times, add buffer events | Automated (future) |
| 2. Inbox Scan | Triage email into tasks | Interactive |
| 3. Morning Sweep | Classify tasks, dispatch agents | Interactive |
| 4. Time Block | Convert tasks to calendar blocks | Interactive |

## Installation

### Prerequisites

```bash
# Google Workspace CLI
npm install -g @anthropic/gws-cli
gws auth login

# Claude Code
# Already installed if you're reading this
```

### Add to Project

Option 1: Symlink skills to your project:

```bash
ln -s /path/to/claude-code-cos/.claude/skills/* ~/.claude/skills/
```

Option 2: Reference in your project's CLAUDE.md:

```markdown
## Chief of Staff Integration

COS skills available at: /Users/Work/Coding/claude-code-cos/.claude/skills/

- `/cos inbox-scan` — Triage Gmail into tasks
- `/cos briefing` — Morning briefing with email stats
- `/cos time-block` — Block time for tasks on calendar
```

## Skills

### `/inbox-scan`

Triage unread emails into:
- **Actionable** — create task
- **Reference** — clip to vault
- **Delegate** — assign to person
- **Archive** — mark read

```bash
/inbox-scan        # Full interactive triage
/inbox-scan quick  # Summary only
/inbox-scan 10     # Scan last 10 emails
```

### `/cos briefing`

Morning briefing with:
- Email inbox count
- Calendar today
- Pending tasks
- Suggested actions

### `/cos time-block`

Convert tasks to calendar events:
1. Read today's priorities
2. Find available time slots
3. Propose time blocks
4. Create calendar events via `gws calendar add`

## Configuration

### Environment Variables

```bash
# .env or shell profile
export GWS_DEFAULT_CALENDAR="primary"
export COS_TIMEZONE="America/Mexico_City"
```

### Auto-Archive Rules

Edit `config/filters.json`:

```json
{
  "archive": [
    { "from": "*@newsletter.*" },
    { "from": "*@noreply.*" },
    { "subject": "*unsubscribe*" }
  ],
  "important": [
    { "from": "boss@company.com" },
    { "from": "*@client.com" }
  ]
}
```

## Integration

### With PersonalOS

COS writes tasks to your vault's daily note:

```markdown
## Tasks from Email

- [ ] Review proposal — from john@client.com
- [ ] Schedule call — delegate to [[Sarah Bradford]]
```

Run `/process-inbox` in PersonalOS to file any reference notes created.

### Standalone

COS can work without a vault — outputs to stdout or creates local task files.

## Philosophy

- **Interactive by default** — human approves before actions
- **Read-only safe** — never sends emails or deletes calendar events
- **Composable** — each skill works standalone or in chains
- **Vault-agnostic** — works with any note system

## License

MIT
