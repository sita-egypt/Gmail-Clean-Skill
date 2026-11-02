# Gmail Clean Skill

A Claude skill that tidies a Gmail inbox without ever archiving or deleting anything. It runs three passes:

1. **Company labels**: tags each inbox thread with the label of the company it's about (client, client group, or partner), picking only from labels you already have.
2. **Review label**: adds `_review` to low-value mail such as sign-in codes, OTPs, no-reply notifications, out-of-office replies and newsletters.
3. **Mark as read**: marks as read the messages that weren't sent directly to you (you're only in CC/BCC, or they arrived through a group or alias like `help@` or `info@`).

Everything stays in the inbox. The skill doesn't create, rename or delete labels, and doesn't archive, trash, spam, send or draft mail.

## Requirements

- Claude with skills enabled (Claude.ai, the Claude desktop app, or Claude Code).
- The **Gmail connector**, connected to the mailbox you want to clean. The skill uses `list_labels`, `search_threads`, `get_thread`, `label_thread` and `unlabel_message`.
- A Gmail label named exactly **`_review`**. Without it, pass 2 is skipped.
- Your company labels already created in Gmail (for example `Example Client`, `Acme`, `Partners/Example Vendor`).

## Installation

### Claude.ai / Claude desktop app

1. Download this repository, or just the `gmail-clean` folder.
2. Zip the `gmail-clean` folder so that `SKILL.md` is inside it.
3. Go to **Settings → Capabilities → Skills**, choose **Upload skill**, and pick the zip.
4. Make sure the Gmail connector is connected.

### Claude Code

Copy the folder into your skills directory:

```bash
git clone https://github.com/sita-egypt/Gmail-Clean-Skill.git
```

```bash
cp -r Gmail-Clean-Skill/gmail-clean ~/.claude/skills/
```

## Usage

Ask Claude something like:

- "Clean up my Gmail inbox"
- "Run gmail-clean"
- "Label my inbox by company and mark CC mail as read"

Claude works through the three passes and finishes with a short report: label counts, what got `_review`, what was marked read, borderline emails it skipped, and anything it couldn't match to a company.

See [HELP.md](HELP.md) for the full rules, customisation and troubleshooting.

## Customising

Two sections in [`gmail-clean/SKILL.md`](gmail-clean/SKILL.md) ship as placeholders for you to edit:

- **Known mappings** link client names and email domains to your labels. Mappings whose label doesn't exist in your account are ignored.
- **Standing rules** are searches for recurring automated mail (monitoring alerts, scheduled reports, vendor reminders) that always gets `_review`.

## Repository layout

```
gmail-clean/
  SKILL.md   the skill itself
README.md    this file
HELP.md      detailed help and troubleshooting
```
