# Module — Campaign Builder

> Generates multi-post campaigns with a shared objective, narrative arc, and CTA path. 7-day, 14-day, and 30-day templates.

---

## What it does

- Picks the campaign type from the 15 in `system/campaign-framework.md`.
- Maps the audience journey (day-0 mental state → day-N).
- Defines the post sequence with role per post.
- Sets the primary CTA path.
- Lists campaign assets needed.
- Surfaces risks.

## When to use it

- Product / feature / brand launches (7-14 days).
- Authority pushes (30 days).
- Proof drops (7 days).
- Waitlist builds (14-30 days).
- Objection-handling sequences (14 days).

## When NOT to use it

- A single post would carry the message faster.
- The brand can't commit to the cadence.
- You don't have proof for the middle posts.

---

## Required inputs

- Active Brand Pack at `brands/[slug]/`.
- Campaign type (or `auto`).
- Duration (7 / 14 / 30 days).
- Objective.
- Primary CTA.

## Optional inputs

- Launch / event date.
- Pre-existing assets (case studies, founder essays, proof drops scheduled).
- Channel scope.
- Audience journey notes (what they currently believe).

---

## Output

```
# Campaign (type, duration, objective, audience, CTA path)
# Audience journey (day-0 → day-N, friction)
# Post sequence (per post: format, angle, hook, body summary, CTA, channel)
# Campaign assets needed
# Distribution
# Risks
# Quality checklist
# Missing Inputs / Proof Needed
```

JSON: `schemas/campaign.schema.json`.

---

## Files in this module

- `README.md` — this file
- `instructions.md` — module system prompt
- `campaign-types.md` — the 15 types catalog
- `launch-campaign-template.md` — 14-day launch template
- `authority-campaign-template.md` — 30-day authority template
- `proof-campaign-template.md` — 7-day proof template
- `quality-checklist.md` — module-specific checks

---

## Related

- Prompt: [`prompts/07-generate-campaign.md`](../../prompts/07-generate-campaign.md)
- System: [`system/campaign-framework.md`](../../system/campaign-framework.md)
- Schema: [`schemas/campaign.schema.json`](../../schemas/campaign.schema.json)

## Mode support

| Mode | Supported? |
|---|---|
| Claude Project Manual | Yes |
| API Production | Yes |
| Renderable Creative Worker | n/a (campaign is a plan; individual posts may be renderable) |
