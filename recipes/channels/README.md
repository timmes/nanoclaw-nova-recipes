# Recipe: Channels + Scheduled Tasks

**Status:** planned — content coming with LinkedIn post 5.

## What it does

NanoClaw's channels aren't just for inbound messages. Scheduled tasks target a `chat_jid`, so Nova can post into a WhatsApp group on a cron without anyone having to message first. That's what makes the morning briefing, weekly briefing, newsletter digest, and grocery-planner feel like they "just happen."

This recipe is the pattern, not a specific flow: how to wire a new scheduled task so it posts to the right group, runs in the right group's container context, and has access to the right mounts.

## Files (to come)

- `scheduled-task-patterns.md` — annotated examples (cron syntax, picking the right `group_folder` vs `chat_jid`, one-off vs recurring)
- Notes on the group-vs-DM distinction (same-looking JIDs, very different contexts)

## Prerequisites

- NanoClaw installed with at least one channel registered (WhatsApp, Telegram, Slack, Discord, or Emacs)
