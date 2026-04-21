# Recipe: Daily Newsletter Digest

**Status:** planned — content coming with LinkedIn post 1.

## What it does

Every day at 05:00 local, Nova:

1. Fetches newsletter-labelled emails from Gmail.
2. Ranks and dedupes items (against yesterday's digest, to avoid duplicates).
3. Writes a markdown digest to a public GitHub repo.
4. Renders a branded HTML email from a template and sends it.
5. Posts a WhatsApp summary with the top 3 headlines.

Skips the whole flow if the inbox is empty or fewer than 3 items survive dedup.

## Files (to come)

- `CLAUDE.md.snippet` — the full "Daily Newsletter Digest" section with step-by-step instructions
- `NovaNewsletterDigest.html` — the email template (sanitized)
- `newsletter-favorites.json.example` — boosted sources + topic weights
- `newsletter-trends.json.example` — rolling trend tracking
- `scheduled-task.yaml` — the cron (`0 5 * * *`) + prompt

## Prerequisites

- NanoClaw installed
- Gmail integration (`/add-gmail` skill merged)
- `html2md` tool in the container
- A GitHub repo to push digests to, plus a `github-token.txt` mounted at `/workspace/secrets/`
- An email mount for the HTML template
