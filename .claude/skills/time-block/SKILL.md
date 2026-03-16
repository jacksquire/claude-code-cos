---
name: time-block
description: "Convert tasks to calendar time blocks"
user-invocable: true
---

# Time Block Skill

Convert prioritized tasks into calendar events for focused execution.

## Activation

- `/cos time-block` — Interactive time blocking
- `/cos time-block [task]` — Block time for specific task

---

## Workflow

### Step 1: Get Today's Priorities

Ask for tasks or read from daily note Big 3:

```
What tasks do you want to time block today?

Or I can read from your daily note's Big 3.
```

### Step 2: Check Calendar Availability

```bash
gws calendar today
```

Identify available blocks:
- Exclude existing meetings
- Apply buffer preferences
- Mark deep work windows

### Step 3: Propose Time Blocks

```
Time Block Proposal
===================

Task: Write project proposal
Estimated: 2 hours
Proposed: 10:00 - 12:00 (before lunch)

Task: Review pull requests
Estimated: 1 hour
Proposed: 14:00 - 15:00 (after lunch)

Task: Email follow-ups
Estimated: 30 min
Proposed: 16:30 - 17:00 (end of day)

Confirm? (y/n/adjust)
```

### Step 4: Create Calendar Events

```bash
gws calendar add "Deep Work: Write proposal" --time "10:00" --duration "2h"
gws calendar add "Deep Work: Review PRs" --time "14:00" --duration "1h"
gws calendar add "Admin: Email follow-ups" --time "16:30" --duration "30m"
```

### Step 5: Confirm

```
Time blocks created:

10:00 - 12:00  Deep Work: Write proposal
14:00 - 15:00  Deep Work: Review PRs
16:30 - 17:00  Admin: Email follow-ups

Your calendar is now blocked for focused work.
```

---

## Task Duration Estimation

Default estimates by task type:

| Type | Default Duration |
|------|-----------------|
| Writing/Creating | 2 hours |
| Review/Reading | 1 hour |
| Email/Admin | 30 min |
| Meetings prep | 15 min |
| Planning | 1 hour |

Ask to adjust if estimate seems wrong.

---

## Preferences

```json
{
  "deep_work_label": "Deep Work:",
  "admin_label": "Admin:",
  "default_duration_minutes": 60,
  "min_block_minutes": 30,
  "buffer_between_blocks": 15
}
```
