# Recipe: Family Expense Tracking

**Status:** planned — content coming with LinkedIn post 7.

## What it does

Any family member snaps a receipt and sends it to Nova via WhatsApp. Nova:

1. OCRs the receipt (via the image-vision skill).
2. Extracts merchant, total amount (EUR), category (Food & Drinks, Shopping, Travel, Services, Entertainment, Health, Transport, Misc.), and date.
3. Appends a row to the running monthly markdown file in the Obsidian vault: `02_Family/Expenses/YYYY/YYYY-MM.md`.
4. Replies with a one-line confirmation.

Running total for the month is always visible in the Obsidian vault, and gets rolled up in the Sunday [briefing](../briefings/).

## Files (to come)

- `CLAUDE.md.snippet` — receipt-photo handling procedure (trigger, extraction, append-to-vault)
- `expenses-template.md.example` — blank month file with table header
- `README.md` screenshot section — WhatsApp conversation showing the ingestion round-trip

## Prerequisites

- NanoClaw installed with image-vision skill merged (`/add-image-vision`)
- Obsidian vault mounted read-write
