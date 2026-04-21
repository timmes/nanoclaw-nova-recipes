# Recipe: Daily Newsletter Digest

## What it does

Every day at 05:00 local, Nova:

1. Fetches newsletter-labelled emails from Gmail.
2. Ranks and dedupes items (against yesterday's digest, to avoid duplicates).
3. Writes a markdown digest to a public GitHub repo.
4. Renders a branded HTML email from a template and sends it.
5. Posts a WhatsApp summary with the top 3 headlines.

**Skips the whole flow** if the inbox is empty or fewer than 3 items survive dedup — no empty email, no empty commit.

## Files

| File | Purpose |
| --- | --- |
| [`CLAUDE.md.snippet`](CLAUDE.md.snippet) | Paste into your group's `CLAUDE.md`. The full 10-step procedure, with `<your-github-user>`, `<your-digest-repo>`, `<you@example.com>` placeholders to replace. |
| [`NovaNewsletterDigest.html`](NovaNewsletterDigest.html) | Email template (Inter Tight / Newsreader / JetBrains Mono). Contains `href="#"` placeholders that Nova fills in — the snippet tells her which URLs go where. |
| [`newsletter-favorites.json.example`](newsletter-favorites.json.example) | Sources you want boosted + topic weights. Rename to `newsletter-favorites.json` in your digest folder. |
| [`newsletter-trends.json.example`](newsletter-trends.json.example) | Starter schema for the rolling 30-day trend tracker. Start from this empty shape; Nova grows it. |
| [`scheduled-task.yaml`](scheduled-task.yaml) | The cron entry + prompt you add to `scheduled_tasks`. |

## Install

1. **Merge the required skill branches** into your NanoClaw fork:
   - `/add-gmail` — Gmail tool (list, read-html, archive, send)
   - `html2md` tool is already in the container at `/app/container/tools/html2md`

2. **Create a public GitHub repo** for the digest archive (e.g. `daily-newsletter-digest`). Generate a personal access token with `contents:write` and save it to `<nanoclaw>/secrets/github-token.txt`.

3. **Prepare a digest folder on the host.** I use `/Users/Shared/Daily-Newsletter-Digest/`. Initialize it as a git repo pointing at the GitHub repo from step 2. Put `newsletter-favorites.json` (copied from the example and edited) and an empty `newsletter-trends.json` (the example shape) inside.

4. **Prepare a templates folder on the host** (e.g. `/Users/Shared/templates/`) and drop `NovaNewsletterDigest.html` into it.

5. **Mount both into your group's container.** Update the group's `container_config.additionalMounts`:
   ```json
   [
     { "hostPath": "/Users/Shared/Daily-Newsletter-Digest", "containerPath": "newsletter", "readonly": false },
     { "hostPath": "/Users/Shared/templates",               "containerPath": "templates", "readonly": true  }
   ]
   ```
   (NanoClaw prefixes `containerPath` with `/workspace/extra/`, so these land at `/workspace/extra/newsletter` and `/workspace/extra/templates`.)

6. **Paste `CLAUDE.md.snippet`** into your group's `CLAUDE.md`. Replace the three placeholders.

7. **Insert the scheduled task** from `scheduled-task.yaml` into `store/messages.db:scheduled_tasks`. Replace `<your-group-folder>` and `<your-chat-jid>`.

8. **Restart NanoClaw** so the new mounts take effect.

## Prerequisites

- NanoClaw installed, running, and a WhatsApp (or other) channel registered
- Gmail OAuth configured (part of `/add-gmail`)
- A public GitHub repo + personal access token with write access
