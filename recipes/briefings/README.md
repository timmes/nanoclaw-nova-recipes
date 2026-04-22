# Recipe: Morning + Weekly Briefings

## What it does

**Morning briefing** — every day at 07:00 local, Nova posts a single WhatsApp message to the family group covering:

- Today's calendar events (from Google Calendar via the `gcal` container tool)
- Kids' school / kindergarten notes relevant today (read from the Obsidian vault)
- Weather for your location
- Up to 3 time-sensitive open items from the vault

**Weekly briefing** — every Sunday at 18:00 local, Nova posts a weekly look-ahead:

- The next 7 days of calendar events, grouped by day
- Scheduling conflicts
- A "for the kids" section
- Open follow-ups from the Decision Log
- One small suggestion to make the week run smoother
- Month-to-date expense total (summed from the vault's monthly expense file — see the [expense-tracking](../expense-tracking/) recipe)

Both are delivered as a **single** message. After sending, Nova silently files the briefing to the vault at `02_Family/Briefings/YYYY/MM/` — no "done ✅" follow-up, no noise.

## Files

| File | Purpose |
| --- | --- |
| [`CLAUDE.md.snippet`](CLAUDE.md.snippet) | Paste into your group's `CLAUDE.md`. Contains both briefing sections with `Kid1`, `Kid2`, and `<your-postcode> <your-city>, <your-country>` placeholders to replace. |
| [`scheduled-tasks.yaml`](scheduled-tasks.yaml) | Two cron entries (`0 7 * * *` and `0 18 * * 0`) to add to `store/messages.db:scheduled_tasks`. |
| [`gcal`](gcal) | **Not part of NanoClaw.** A self-contained Node.js CLI wrapping the Google Calendar API with a service-account JWT. Drop it into `container/tools/` in your NanoClaw fork and rebuild the container. No npm deps. |

## Install

1. **Install the `gcal` container tool.** It is *not* part of NanoClaw — copy [`gcal`](gcal) from this recipe into `<nanoclaw>/container/tools/` (make it executable) and rebuild the container with `./container/build.sh`. NanoClaw's Dockerfile copies `container/tools/*` into `/usr/local/bin/`, so after rebuild the `gcal` command is on `PATH` inside every agent container. No npm install needed — it talks to the Google Calendar REST API directly over `https` with a service-account JWT.

   You also need:
   - A Google Cloud service account with the Calendar API enabled
   - The service account JSON key saved to `<nanoclaw>/secrets/gcal-key.json` (mounted into the container at `/workspace/secrets/gcal-key.json`)
   - The service account's email invited to your family Google Calendar as a writer (Calendar → Settings → Share with specific people)
   - The calendar ID exported via `GCAL_CALENDAR_ID` (default: `primary`)

2. **Mount your Obsidian vault** into the group's container. Update the group's `container_config.additionalMounts`:
   ```json
   [
     { "hostPath": "/path/to/your/vault", "containerPath": "vault", "readonly": false }
   ]
   ```
   (NanoClaw prefixes `containerPath` with `/workspace/extra/`, so this lands at `/workspace/extra/vault`.)

   The snippet assumes this vault structure — adapt paths if yours differs:
   ```
   vault/
     00_Start/DecisionLog.md
     02_Family/Briefings/YYYY/MM/YYYY-MM-DD.md
     02_Family/Expenses/YYYY/YYYY-MM.md
     Kids/Kid1/School/Events.md
     Kids/Kid1/Kindergarten/Overview.md
     Kids/Kid2/Kindergarten/Overview.md
   ```

3. **Paste [`CLAUDE.md.snippet`](CLAUDE.md.snippet)** into your group's `CLAUDE.md`. Replace `Kid1`, `Kid2`, and the location placeholder. Adapt vault paths to match step 2.

4. **Insert both rows from [`scheduled-tasks.yaml`](scheduled-tasks.yaml)** into `store/messages.db:scheduled_tasks`. Replace `<your-group-folder>` and `<your-chat-jid>`.

5. **Restart NanoClaw** so the new mount takes effect. The scheduler polls `scheduled_tasks` every 60 seconds — no restart is needed for scheduled task changes alone, but the mount change requires one.

## The single-message rule

NanoClaw's router auto-sends any final agent response text as a WhatsApp message. If you don't explicitly tell Nova to stay silent after the first message, you'll get a second "Here's your briefing ✅" message every time. Add something like this to your group's `CLAUDE.md` under "What I never do":

> **For scheduled tasks (briefings): I send EXACTLY ONE message via the send_message tool. After that, I do remaining work (saving files) silently. I MUST NOT produce any final response text — no "Done", no confirmation, no summary.**

## Why the vault write happens *after* the message

Obsidian sync is eventually consistent. Writing to the vault before sending means a slow fsync can delay the 07:00 ping. Sending first, writing second keeps the user-facing latency tight and the file write is fire-and-forget.

## Prerequisites

- NanoClaw installed, running, and a WhatsApp (or other) channel registered
- `gcal` dropped into `container/tools/` (from this recipe — it is not part of NanoClaw) + a Google service account with Calendar API access
- Obsidian (or any markdown-based) vault mounted into the group's container
- (Optional) The [expense-tracking](../expense-tracking/) recipe installed if you want the `💸 EXPENSES` line in the weekly briefing to be non-empty
