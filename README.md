# nanoclaw-nova-recipes

Real-world customizations for [NanoClaw](https://github.com/qwibitai/nanoclaw) — the personal Claude assistant that lives in a container and talks to you on WhatsApp (and other channels). These are the recipes behind **Nova**, my family's assistant: what it does every day, the `CLAUDE.md` sections that drive it, the scheduled tasks that trigger it, and the templates it uses.

None of this replaces NanoClaw itself. Install NanoClaw first (see upstream), then cherry-pick recipes from here.

## Recipes

| Recipe | What it does |
| --- | --- |
| [newsletter-digest](recipes/newsletter-digest/) | Reads newsletters from Gmail at 5am, ranks and dedupes, emails a clean HTML digest, pushes markdown to a public repo. |
| [briefings](recipes/briefings/) | Morning briefing (calendar + kids' schedules) on WhatsApp every day; weekly briefing with expenses roll-up on Sundays. |
| [grocery-planner](recipes/grocery-planner/) | Downloads German retailer flyers (ALDI/REWE/LIDL/Penny), builds a 7-day meal plan + shopping list, writes it to the Obsidian vault. |
| [expense-tracking](recipes/expense-tracking/) | Family members send receipt photos via WhatsApp. Nova extracts merchant/amount/category/date and appends to the monthly vault file. Running totals visible anytime. |
| [voice-vision](recipes/voice-vision/) | Voice notes get transcribed. Photos (receipts, flyers, appliance labels) get OCR'd and routed to the right handler. |
| [channels](recipes/channels/) | Patterns for scheduled tasks that post to a WhatsApp group on a cron — without the group needing to message first. |

## How a recipe is structured

Each folder contains some combination of:

- `README.md` — what it does, prerequisites, how to install, mounts required
- `CLAUDE.md.snippet` — the section to paste into your group's `CLAUDE.md`
- `scheduled-task.yaml` — the cron entry (schedule + prompt) to add to `scheduled_tasks`
- Example/template files (HTML templates, markdown schemas, config samples with placeholders)

## Relationship to upstream NanoClaw

Recipes target upstream NanoClaw + its skill branches (e.g. `skill/image-vision`, `skill/voice-transcription`). If a recipe needs a specific skill branch merged, its README says so. Nothing here patches or forks NanoClaw itself.

## Status

Work in progress. Each recipe folder currently contains a stub README — content lands as the LinkedIn series goes out.
