# Prompt — 03 Generate Content Strategy

> Generates content pillars, recurring series, and a 30/90-day plan tied to the brand's offer.

---

## Prompt

```
You are operating PostStudio System.

Generate a content strategy using:
- modules/content-strategy-os/instructions.md
- system/content-system.md
- system/carousel-framework.md
- system/campaign-framework.md
- system/safety-and-claims-rules.md
- The active Brand Pack at brands/[BRAND_SLUG]/

# Inputs

- Brand: brands/[BRAND_SLUG]/
- Time window: [30 / 90 days]
- Channel scope: [linkedin / instagram / tiktok / x / multi]
- Cadence: [posts/week per channel]
- Upcoming launches / events / proof drops: [list]
- Recent post results: [what worked / didn't, if known]
- Constraints for this period: [banned topics / required topics]

# Process

1. Define 3-5 content pillars per modules/content-strategy-os/content-pillars-template.md.
2. Define 2-4 recurring series per modules/content-strategy-os/recurring-series-template.md.
3. Build the 30-day plan per modules/content-strategy-os/content-map-template.md.
4. Sketch a 90-day direction (campaign placement, pillar emphasis shifts).
5. Map content to offer (each pillar's path to conversion).
6. Plan distribution per channel.
7. Surface gaps (missing inputs, proof needed, brand pack fields requiring refresh).

# Output

See modules/content-strategy-os/ templates. Markdown for Mode 1; JSON conforming to schemas/content-strategy.schema.json for Mode 2.

End with:
- Quality Checklist (modules/content-strategy-os/quality-checklist.md)
- Missing Inputs / Proof Needed
- Recommended next prompt (typically prompts/04-generate-carousel.md for the first post in the plan)

# Constraints

- 3-5 pillars max.
- 2-4 recurring series max.
- Every plan entry tied to a pillar.
- No invented stats or audience-size claims.
- Don't recommend cadences the brand can't maintain.
```

---

## Example invocation

```
Brand: brands/lumen/
Time window: 30 days
Channel scope: linkedin primary, instagram secondary
Cadence: 4 posts/week LinkedIn, 2/week IG
Upcoming launches: v3.4 Focus Mode (day 13)
Recent post results: founder confessional carousels outperform listicles 3x
Constraints for this period: avoid generic "AI productivity" topics
```

Claude returns the strategy.

---

## When to use

- After a Brand Pack is built.
- Quarterly to refresh the plan.
- Before any campaign.

---

## Iterating

```
Re-plan week 4 to lead with the launch instead of authority posts. Move the launch posts from days 13-14 to day 22-30.
```
