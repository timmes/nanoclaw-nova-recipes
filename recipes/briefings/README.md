# Recipe: Morning + Weekly Briefings

**Status:** planned — content coming with LinkedIn post 2.

## What it does

**Morning briefing (daily, 07:00 local, WhatsApp group)** — a single message covering:

- Today's calendar events (from Google Calendar via `gcal`)
- Kids' school / kindergarten items for the day
- Anything logged in the vault's Decision Log that needs a nudge

**Weekly briefing (Sunday, 18:00 local, WhatsApp group)** — everything above plus:

- Next week's calendar at a glance
- Month-to-date expense total (summed from the vault's monthly expenses file — see [expense-tracking](../expense-tracking/))

Both are single messages. No follow-up "done" messages. Silent file writes happen after.

## Files (to come)

- `CLAUDE.md.snippet` — the morning + weekly briefing sections
- `scheduled-tasks.yaml` — both cron entries (`0 7 * * *` and `0 18 * * 0`)

## Prerequisites

- NanoClaw installed
- Google Calendar via the `gcal` container tool
- Obsidian vault mounted (for kids' schedules + expense logs)
