# Module — Reels Script Machine

> Generates short-form video scripts (Reels, TikTok, Shorts, X video). Hook + beats + on-screen text + B-roll directions + caption + CTA.

---

## What it does

- Picks the script type (talking-head / faceless / B-roll-led / hybrid).
- Picks the duration (15s / 30s / 45s / 60s / 90s).
- Generates the full script + on-screen text timeline + B-roll directions.
- Generates the caption + CTA.
- Runs the short-video quality checklist.

## When to use it

- For Reels, TikTok, YouTube Shorts, X video.
- When the brief is short-form video, not carousel.

## When NOT to use it

- For carousels → use [`carousel-machine`](../carousel-machine/).
- For long-form essays / posts → use a different output format.

---

## Required inputs

- Active Brand Pack at `brands/[slug]/`.
- Topic.
- Audience.
- Platform.
- Goal.
- Script type (or `auto`).
- Duration (or `auto`; default 30s).

## Optional inputs

- Voice owner (founder name, operator name, brand voice).
- B-roll constraints (Brand Pack `constraints.md` may forbid faces).
- Reference reels.

---

## Output

```
# Strategic Angle
# Script (type, duration, word count, beats with voice + visual + audio)
# On-screen text (timed)
# B-roll list
# Caption
# CTA
# Visual style
# Editing notes
# Quality checklist
# Missing Inputs / Proof Needed
```

JSON form: conforms to `schemas/reels-script.schema.json`.

---

## Files in this module

- `README.md` — this file
- `instructions.md` — module system prompt
- `script-structures.md` — durations and beat structures
- `b-roll-directions.md` — how to direct B-roll
- `output-template.md` — the standard output shape
- `quality-checklist.md` — module-specific checks

---

## Related

- Prompt: [`prompts/06-generate-reels-script.md`](../../prompts/06-generate-reels-script.md)
- System: [`system/reels-framework.md`](../../system/reels-framework.md)
- Schema: [`schemas/reels-script.schema.json`](../../schemas/reels-script.schema.json)

## Mode support

| Mode | Supported? |
|---|---|
| Claude Project Manual | Yes |
| API Production | Yes |
| Renderable Creative Worker | n/a (script-only; not a render target) |
