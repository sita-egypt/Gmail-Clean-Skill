# Gmail Clean: Help

## What the skill does

| Pass | What it changes | What it never does |
|---|---|---|
| 0. Setup | Nothing. Finds your address from your Sent mail, loads your labels and finds `_review`. | Create labels |
| 1. Company labels | Adds an existing company label to inbox threads | Create new labels, remove labels |
| 2. Review label | Adds `_review` to unimportant mail | Archive or delete |
| 3. Mark as read | Removes `UNREAD` from messages where you're not in **To** | Touch messages addressed To you |

The only label the skill ever removes is the system label `UNREAD`.

## Before the first run

1. **Connect Gmail** in Claude. If the connector is missing, the skill stops and asks you to connect it.
2. **Create a `_review` label** in Gmail (Settings → Labels → Create new label). The name must be exactly `_review`.
3. **Create your company labels.** The skill only uses labels that already exist.
4. **Edit the Known mappings and Standing rules** in `gmail-clean/SKILL.md` to match your clients, domains and recurring automated mail. They ship as placeholders.

## How companies are matched (pass 1)

In this order:

1. A client or company name in the subject or snippet.
2. Sender or recipient domains, excluding your own company's domain.
3. A matching subject in another thread that names the client (for example a ticket reply `RE: <subject> [ ref:... ]`).
4. If still unclear, the quoted original message inside the thread.

Partners and vendors are labelled by the client being discussed. The partner's own label (e.g. `Partners/Example Vendor`) is used only when the thread is about the partner itself or covers many clients. Labels for old or inactive accounts (e.g. `z_Old/...`) are used only when they're the company's only label, and the report says so.

Threads that match no existing label are left alone and listed in the report.

## What gets `_review` (pass 2)

**Always**, even on company-labelled threads, the standing rules. They ship as examples you should edit in `SKILL.md`:

- Uptime-monitor "Monitor is DOWN" alerts (and your out-of-office replies to them)
- Recurring automated reports
- Vendor helpdesk "awaiting your response" reminders, **only** when the thread is a single email

**Otherwise, for unlabelled inbox mail:** sign-in links, OTPs and verification codes, no-reply system notifications, out-of-office replies, recall notices, newsletters and bulk vendor pitches.

**Never:** client, partner or onboarding threads; support tickets; threads you replied to; contracts, invoices, meetings, minutes of meetings.

**Borderline, reported instead of labelled:** "Monitor is UP" alerts, vendor ticket-closing notices, security scans and alerts, Search Console alerts, hosting/billing/payment-failure notices, deadline-driven programme changes, and unclear internal notices.

## What gets marked read (pass 3)

A message is marked read only if your address is **not** in its To field: you were only CC'd or BCC'd, or it reached you through a group or alias. This is decided per message, so a thread can end up partly read.

## Troubleshooting

**"Gmail connector not available"**
Connect Gmail in Claude's connector settings and run the skill again.

**Pass 2 was skipped**
There's no label named exactly `_review`. Create it in Gmail and rerun.

**A thread wasn't labelled**
No existing label matched it. Create the label (or add a mapping to `SKILL.md`) and rerun; the skill only adds labels, so rerunning is safe.

**Something got the wrong label or `_review`**
Remove the label in Gmail. The skill never deletes or archives, so nothing is lost.

**A message I wanted unread was marked read**
Mark it unread in Gmail. Check whether it came through a group or alias, since those are treated as not addressed to you.

## Running it again

The skill is safe to rerun at any time. It only adds labels and removes `UNREAD`, and each standing-rule search skips threads that already carry `_review`.
