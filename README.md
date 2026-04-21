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

## The vault (assumed, not required)

Most recipes read from and write to a shared **Obsidian vault** — a folder full of markdown files that the family edits by hand (Obsidian app, mobile Obsidian, or any text editor) and that Nova also reads/writes via a container mount. The vault is where running state lives: decisions, kids' schedules, expenses, meal plans, home inventory. You don't need Obsidian specifically — any folder of markdown works — but the paths in these recipes assume the structure below.

```
vault/
├── 00_Start/                    # Top of mind — decisions, inbox
│   ├── DecisionLog.md           # "that's a decision" lands here; Nova won't re-ask
│   └── Inbox/                   # Unsorted captures
├── 01_Adults/                   # Per-adult folders (privacy-respecting)
│   ├── Adult1/
│   └── Adult2/
├── 02_Family/                   # Shared family data
│   ├── Briefings/YYYY/MM/       # Morning/weekly briefings get archived here
│   └── Expenses/YYYY/           # Monthly expense ledgers (YYYY-MM.md)
├── 03_Kids/                     # Per-child folders
│   ├── Kid1/School/Events.md
│   ├── Kid1/Kindergarten/Overview.md
│   └── Kid2/Kindergarten/Overview.md
├── 04_Home/                     # Household
│   ├── Grocery/                 # Current Plan.md, Flyers/, Archive/, Price History.json
│   ├── Inventory/Appliances.md  # Photo of an appliance → appended here
│   └── Maintenance/
└── 99_Assets/                   # Images, templates, transcripts
```

**If you don't have a vault yet:** start empty. Create the folders your chosen recipes need (listed in each recipe's `README.md`). Mount the vault root into Nova's container read-write, and either replicate the numbering convention or swap paths in the `CLAUDE.md.snippet` to match your layout. The numbering (`00_`, `01_`, …) is ordering-for-humans — it's not semantic, you can drop it.

**Which recipes depend on the vault?**

| Recipe | Vault paths used |
| --- | --- |
| briefings | `00_Start/DecisionLog.md`, `02_Family/Briefings/`, `02_Family/Expenses/`, `03_Kids/` |
| grocery-planner | `04_Home/Grocery/` |
| expense-tracking | `02_Family/Expenses/` |
| voice-vision | `04_Home/Inventory/Appliances.md` (for appliance photos) |
| newsletter-digest | — (uses a separate GitHub-backed folder, not the vault) |
| channels | — (pattern only) |

## Relationship to upstream NanoClaw

Recipes target upstream NanoClaw + its skill branches (e.g. `skill/image-vision`, `skill/voice-transcription`). If a recipe needs a specific skill branch merged, its README says so. Nothing here patches or forks NanoClaw itself.

## Status

Work in progress. Each recipe folder currently contains a stub README — content lands as the LinkedIn series goes out.
