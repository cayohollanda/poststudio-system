# Module — Content Strategy OS

> Generates content pillars, recurring series, channel strategy, and 30/90-day plans that tie every post to the brand's offer.

---

## What it does

Takes a Brand Pack and returns:

- 3-5 **content pillars** — the brand's top-level argument categories.
- 2-4 **recurring series** — repeating formats with a known cadence.
- A **30-day content plan** — topics tagged by pillar, format, channel.
- A **90-day direction** — pillar emphasis shifts, campaign calendar.
- A **content-to-offer map** — how each pillar feeds the business.
- A **distribution plan** — channels and adaptation per channel.

## When to use it

- After a Brand Pack is built, before any single post is generated.
- Quarterly to refresh the plan.
- Before any campaign (use this output as the strategic anchor).

## When NOT to use it

- For a one-off post outside any strategy. Use `carousel-machine` directly.

---

## Required inputs

- Active Brand Pack at `brands/[slug]/`.
- Time window (default: 30 days; alternatives: 7, 14, 90).
- Channel scope (default: primary platform from Brand Pack).

## Optional inputs

- Recent post results (what worked, what didn't).
- Upcoming launches / events / proof drops.
- Posting cadence preference.
- Banned topics / required topics for the period.

---

## Output

A strategy document with:

- Pillar definitions (with topic clusters per pillar).
- Series definitions (cadence, format, anchor structure).
- 30-day plan (day-by-day or week-by-week, with topic, format, channel, pillar tag).
- 90-day direction.
- Content-to-offer map.
- Distribution plan.

See [`content-pillars-template.md`](content-pillars-template.md), [`content-map-template.md`](content-map-template.md), [`recurring-series-template.md`](recurring-series-template.md).

JSON schema: `schemas/content-strategy.schema.json`.

---

## Files in this module

- `README.md` — this file
- `instructions.md` — the module system prompt
- `content-pillars-template.md` — template for pillar definitions
- `content-map-template.md` — template for the day-by-day plan
- `recurring-series-template.md` — template for series definitions
- `quality-checklist.md` — module-specific checks

---

## Related

- Prompt: [`prompts/03-generate-content-strategy.md`](../../prompts/03-generate-content-strategy.md)
- Schema: [`schemas/content-strategy.schema.json`](../../schemas/content-strategy.schema.json)
- Often paired with: [`prompts/12-create-monthly-content-plan.md`](../../prompts/12-create-monthly-content-plan.md)
- Feeds into: [`carousel-machine`](../carousel-machine/), [`reels-script-machine`](../reels-script-machine/), [`campaign-builder`](../campaign-builder/)

## Mode support

| Mode | Supported? |
|---|---|
| Claude Project Manual | Yes |
| API Production | Yes |
| Renderable Creative Worker | n/a |
