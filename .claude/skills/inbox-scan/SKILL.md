---
name: inbox-scan
description: "Triage Gmail inbox into vault tasks, references, or archive"
user-invocable: true
---

# Inbox Scan Skill

Triage unread emails into actionable tasks, reference notes, or archive. Inspired by Jim Prosser's Chief of Staff Layer 2.

## Activation

- `/inbox-scan` — Full interactive scan
- `/inbox-scan quick` — Quick summary, no actions
- `/inbox-scan [N]` — Scan last N emails (default: 20)

## Prerequisites

- `gws` CLI installed: `npm install -g @anthropic/gws-cli`
- Authenticated: `gws auth login`
- Gmail API enabled in Google Cloud project

---

## Mode: Full Scan (default)

### Step 1: Fetch Unread Emails

```bash
gws gmail +triage
```

Parse output to get list of emails with:
- ID
- Subject
- From (sender)
- Date
- Snippet (preview)

### Step 2: Present Batch for Triage

Show emails in groups of 5 for classification:

```
Email Inbox Scan
================

1. [Subject line truncated at 60 chars]
   From: sender@example.com | 2h ago
   Preview: First 80 chars of email...

2. [Subject line]
   From: sender@example.com | Yesterday
   Preview: ...

For each, classify as:
  (A) Actionable — create task
  (R) Reference — clip to vault
  (D) Delegate — task for someone else
  (X) Archive — mark read, ignore

Enter classifications (e.g., "AARXD"):
```

### Step 3: Process Classifications

For each email based on classification:

#### (A) Actionable
1. Read full email content: `gws gmail read [id]`
2. Extract:
   - Action required
   - Deadline (if mentioned)
   - Related people
3. Create task entry for daily note:
   ```markdown
   - [ ] [Action extracted] — from [sender] (email: [subject])
   ```
4. Optionally create linked note in `00-Inbox/` with full email

#### (R) Reference
1. Read full email: `gws gmail read [id]`
2. Create note in `00-Inbox/email-[slug].md`
3. Frontmatter:
   ```yaml
   ---
   type: resource
   source: email
   from: sender@example.com
   subject: "Original subject"
   received: YYYY-MM-DDTHH:mm:ss
   ---
   ```
4. Body: Email content formatted as markdown

#### (D) Delegate
1. Ask: "Who should handle this?"
2. Create task with person tag:
   ```markdown
   - [ ] [Subject] — delegate to [[Person Name]] (email from [sender])
   ```

#### (X) Archive
1. Mark as read (no vault action)
2. Note in log that email was reviewed

### Step 4: Summary

```
Inbox Scan Complete
===================

Processed: 12 emails

  5 Actionable → tasks added to daily note
  2 Reference → clipped to inbox
  1 Delegate → assigned to [[Person]]
  4 Archived → marked read

Next: Run /morning to review and prioritize tasks
```

---

## Mode: Quick Scan

```bash
gws gmail list --unread
```

Show summary only, no actions:

```
Email Quick Scan
================

12 unread emails

By sender:
  4 - newsletter@company.com (newsletters)
  3 - boss@work.com (might be important)
  2 - noreply@service.com (automated)
  3 - various

Oldest: 3 days ago
Newest: 2 hours ago

Run /inbox-scan to triage
```

---

## Daily Note Integration

### Tasks from Email Section

Add to daily note template a section for email tasks:

```markdown
## Tasks from Email

<!-- Populated by /inbox-scan -->
- [ ] [Task from email]
```

### Or: Append to Existing Tasks

If daily note exists, append email tasks under existing task section with clear grouping:

```markdown
### From Email
- [ ] Review proposal — from john@client.com
- [ ] Schedule call — delegate to [[Sarah Bradford]]
```

---

## Error Handling

### gws Not Installed
```
gws CLI not found. Install with:
  npm install -g @anthropic/gws-cli

Then authenticate:
  gws auth login
```

### Not Authenticated
```
Gmail not authenticated. Run:
  gws auth login
```

### API Error
```
Gmail API error: [error message]
Check your Google Cloud project settings.
```

---

## Configuration

### Email Filters

Common filters to auto-classify:

| Pattern | Auto-Action |
|---------|-------------|
| `from:*@newsletter.*` | Archive |
| `from:*@noreply.*` | Archive (unless contains "confirm") |
| `subject:*unsubscribe*` | Archive |
| `from:[known contacts]` | Actionable |

### Integration with Chief of Staff

The `/chief-of-staff briefing` command should include email stats:

```
Email Status:
├── 12 unread
├── 3 flagged important
└── Oldest: 2 days
→ Run /inbox-scan to triage
```

---

## Important Notes

- This skill reads email but doesn't send or delete
- Marked-as-read happens via Gmail API
- All created notes go to inbox for later filing
- Run `/process-inbox` after to file email notes properly
