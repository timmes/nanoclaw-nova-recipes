# Recipe: Voice + Vision

**Status:** planned — content coming with LinkedIn post 6.

## What it does

**Voice notes** — WhatsApp voice messages get transcribed (OpenAI Whisper API or local whisper.cpp) and treated as typed input. No separate "transcribe this" step; the agent just reads them. Handy for messages dictated while driving, cooking, or with a kid on one hip.

**Photos** — Nova handles photos based on what's in them:

- **Receipts** → extract merchant/amount/category/date, append to monthly expenses file (see [expense-tracking](../expense-tracking/))
- **Retailer flyers** → hand off to the grocery-planner skill for meal planning (see [grocery-planner](../grocery-planner/))
- **Appliances / products** → extract model + serial, ask whether to log to the home inventory
- **Anything else** → describe what's there and ask how to handle it

## Files (to come)

- `CLAUDE.md.snippet` — the "When I receive a voice note" / "When I receive a photo" routing blocks
- Screenshots of a receipt round-trip and a voice-note round-trip

## Prerequisites

- NanoClaw installed
- Voice: `/add-voice-transcription` (OpenAI Whisper) or `/use-local-whisper` (whisper.cpp, Apple Silicon)
- Vision: `/add-image-vision`
