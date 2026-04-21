# Recipe: Grocery Planner

**Status:** planned — content coming with LinkedIn post 3.

## What it does

Every Sunday at 09:00 local, Nova:

1. Downloads this week's ALDI Nord Prospekt PDF (predictable URL, no scraping).
2. If manual-upload stores (REWE, LIDL, Penny, Rossmann) are missing flyers, optionally asks via WhatsApp for PDFs/photos.
3. Reads flyer text, config (budget, dietary rules, cuisine rotation), and price history.
4. Generates a 7-day meal plan obeying hard rules (Wed vegetarian, Fri pizza, Sun batch-cook, kid-friendly).
5. Writes `Current Plan.md` to the Obsidian vault with meal table, shopping list, per-store breakdown.
6. Sends a German WhatsApp summary to the family group.

This recipe is a **container skill** — a `SKILL.md` + scripts in a dedicated folder, mounted into Nova's container.

## Files (to come)

- `SKILL.md` — the full skill definition (triggers, execution flow, output format)
- `config.yaml.example` — family preferences, stores to include, budget
- `scripts/prepare_context.py` — downloads + parses flyers
- `scripts/price_tracker.py` — rolling price history
- `scheduled-task.yaml` — the Sunday cron

## Prerequisites

- NanoClaw installed
- A host directory for skills mounted into the container (see [skills as folders](../../README.md))
- Python 3 + `poppler` (for `pdftotext`), `requests`, `pyyaml`, `python-dateutil` in the container
- Obsidian vault mounted for writing the plan
