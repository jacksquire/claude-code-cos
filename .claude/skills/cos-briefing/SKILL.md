---
name: cos-briefing
description: "Morning briefing with email, calendar, and task overview"
user-invocable: true
---

# COS Briefing Skill

Generate a morning briefing combining email inbox, calendar, and pending tasks.

## Activation

- `/cos briefing` — Full morning briefing
- `/cos briefing quick` — Condensed summary

---

## Mode: Full Briefing

### Step 1: Gather Data

Run in parallel:

```bash
# Email stats
gws gmail +triage

# Calendar agenda
gws calendar +agenda
```

### Step 2: Analyze Email

Parse unread emails:
- Count total unread
- Group by sender domain
- Identify potentially important (known contacts, keywords)
- Note oldest unread age

### Step 3: Analyze Calendar

Parse today's events:
- Count meetings
- Calculate total meeting time
- Identify gaps for deep work
- Note any travel/buffer needs

### Step 4: Generate Briefing

```
Good morning! Here's your briefing for [Day, Month Date]

Email Inbox
===========
12 unread emails
├── 4 newsletters (auto-archive candidates)
├── 3 from work contacts
├── 2 automated notifications
└── 3 other

Oldest: 2 days | Flagged: 1
→ Run /inbox-scan to triage

Today's Calendar
================
5 events | 3.5 hours in meetings

09:00 - 09:30  Team standup
11:00 - 12:00  Client call
14:00 - 15:00  Review meeting

Deep work blocks: 09:30-11:00, 12:00-14:00

Suggestions
===========
→ Process email before first meeting
→ Block deep work time on calendar
```

---

## Mode: Quick Briefing

```
Quick Briefing | Tuesday, March 15
==================================
Email: 12 unread (3 important)
Calendar: 5 events, 3.5h meetings
Next: Team standup in 45 min
```

---

## Integration with PersonalOS

If running in vault context, also show:
- Vault inbox count
- Daily note status
- Overdue tasks from previous days
