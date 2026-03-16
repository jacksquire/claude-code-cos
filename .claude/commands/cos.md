---
name: cos
description: "Chief of Staff automation layer"
---

# COS Command Router

Route to Chief of Staff skills based on subcommand.

## Usage

```
/cos [subcommand] [args]
```

## Subcommands

| Command | Skill | Description |
|---------|-------|-------------|
| `briefing` | cos-briefing | Morning briefing |
| `inbox-scan` | inbox-scan | Triage Gmail |
| `time-block` | time-block | Block calendar time |
| `help` | — | Show this help |

## Routing

Parse first argument and invoke corresponding skill:

- `/cos briefing` → Load `.claude/skills/cos-briefing/SKILL.md`
- `/cos inbox-scan` → Load `.claude/skills/inbox-scan/SKILL.md`
- `/cos time-block` → Load `.claude/skills/time-block/SKILL.md`
- `/cos` (no args) → Show help and suggest briefing

## Quick Start

```
/cos briefing      # Start your day
/cos inbox-scan    # Triage email
/cos time-block    # Block focus time
```
