# Prompt — 07 Generate Campaign

> Generates a multi-post campaign with shared objective, narrative arc, and CTA path. 7 / 14 / 30-day templates.

---

## Prompt

```
You are operating PostStudio System.

Generate a campaign using:
- modules/campaign-builder/instructions.md
- system/campaign-framework.md
- system/content-system.md
- system/safety-and-claims-rules.md
- The active Brand Pack at brands/[BRAND_SLUG]/

# Inputs

- Brand: brands/[BRAND_SLUG]/
- Campaign type: [from the 15 in modules/campaign-builder/campaign-types.md, or "auto"]
- Duration: [7 / 14 / 30 days]
- Objective: [one sentence]
- Primary CTA: [save | comment | share | click | dm | follow | try]
- Audience: [role + situation]
- Channel scope: [linkedin | instagram | tiktok | x | multi]
- Pre-existing assets: [scheduled events, founder essays already drafted, proof drops]
- Audience day-0 mental state: [what they currently believe]
- Audience day-N mental state: [what they should believe by the end]

# Process

1. Pick (or honor) the campaign type per modules/campaign-builder/campaign-types.md.
2. Map the audience journey (day-0 → day-N, friction).
3. Build the 5-act arc (Setup → Pressure → Reframe → Proof → Ask) across the duration.
4. For each post:
   - Date / day number.
   - Format (carousel / reel / essay / case study).
   - Angle (from system/content-system.md).
   - Hook.
   - Body summary.
   - CTA.
   - Platform priority.
   - Proof needed.
5. Identify campaign assets needed (stories, screenshots, numbers, founder essays).
6. Plan distribution.
7. List risks.

# Output

Markdown per the appropriate template:
- launch / 7-14-day → modules/campaign-builder/launch-campaign-template.md
- 30-day authority → modules/campaign-builder/authority-campaign-template.md
- 7-day proof → modules/campaign-builder/proof-campaign-template.md

JSON per schemas/campaign.schema.json (Mode 2).

End with:
- Quality Checklist (modules/campaign-builder/quality-checklist.md)
- Missing Inputs / Proof Needed

# Constraints

- Single named primary CTA.
- Each post has a distinct role in the 5-act arc.
- No two posts argue the same point with different words.
- Save explicit CTA for acts 4-5.
- Honor brand constraints.md (banned topics / off-limits competitors).
```

---

## Example invocation

```
Brand: brands/lumen/
Campaign type: product-launch
Duration: 14 days
Objective: drive 50 trial signups for v3.4 Focus Mode
Primary CTA: try
Audience: senior PMs and engineering leads at 50-500 person SaaS
Channel scope: linkedin primary, instagram secondary
Pre-existing assets: Mira's founder essay drafted; one anonymized case study ready
Audience day-0 mental state: skeptical of "AI productivity" tools after over-installing
Audience day-N mental state: convinced the structural-not-behavioral fix is the unlock
```

Claude returns the 14-day campaign with day-by-day post sequence.

---

## When to use

- Launches, authority pushes, proof drops, waitlists, objection-handling sequences.
- A single post is the wrong unit.

---

## When NOT to use

- A single post would carry the message faster.
- The brand can't commit to the cadence.
- You don't have proof for the middle posts.

---

## Iterating

```
Compress to 7 days. Drop the authority arc.
```

```
Replace day 11's data drop with a founder reflection. We don't have the methodology cleared yet.
```
