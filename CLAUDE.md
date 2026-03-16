# Claude Code Chief of Staff

Personal automation layer for email triage, calendar management, and task orchestration.

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| inbox-scan | `/cos inbox-scan` | Triage Gmail into tasks |
| cos-briefing | `/cos briefing` | Morning briefing with email/calendar |
| time-block | `/cos time-block` | Block calendar time for tasks |

## Prerequisites

```bash
npm install -g @anthropic/gws-cli
gws auth login
```

## Integration

This repo provides automation skills that can be used standalone or integrated with other systems like PersonalOS.

### Standalone Usage

```bash
cd /Users/Work/Coding/claude-code-cos
claude
/cos briefing
```

### With PersonalOS

Add symlinks or reference skills from PersonalOS vault.

## Architecture

Based on Jim Prosser's 4-layer Chief of Staff:

1. **Calendar Transit** — (future) Drive time calculations
2. **Inbox Scan** — Email triage into actionable tasks
3. **Morning Sweep** — Task classification and prioritization
4. **Time Block** — Calendar blocking for focused work

## Configuration

Edit `config/filters.json` for:
- Auto-archive email rules
- Important sender list
- Work hours and timezone
- Time blocking preferences
