# Prompt — 12 Create Monthly Content Plan

> Generates a 30-day content plan with day-by-day post details. The deliverable the client receives at the start of each month.

---

## Prompt

```
You are operating PostStudio System.

Generate a monthly content plan using:
- modules/content-strategy-os/instructions.md
- modules/agency-delivery-kit/monthly-content-plan-template.md
- system/campaign-framework.md
- The active Brand Pack at brands/[BRAND_SLUG]/, especially content-pillars.md

# Inputs

- Brand: brands/[BRAND_SLUG]/
- Month: [YYYY-MM]
- Theme: [one-sentence theme of the month]
- Total posts: [N — typically 16-20 for a 30-day month with 4-5 posts/week]
- Channels: [linkedin / instagram / tiktok / x / multi]
- Active campaigns this month: [list with date ranges]
- Recurring series running: [from brand's series definitions]
- Required client inputs already gathered: [from input-collection-checklist.md]
- Off-limits topics for the period: [from constraints.md updates]

# Process

1. Anchor the plan to the brand's content pillars (no untagged posts).
2. Place active campaigns first (they have fixed date ranges).
3. Place recurring series at their fixed days.
4. Fill remaining slots with pillar-driven topics.
5. Balance:
   - Pillar coverage (don't over-index one).
   - Format mix (carousels, reels, founder essays, single images).
   - TOFU vs MOFU/BOFU (audience funnel coverage).
   - Heavy / light week alternation.
6. For each post:
   - Day, date, topic (category, not full title), format, pillar, channel, primary CTA, status, notes.
7. List required client inputs for the month.
8. List risks.
9. Define success metrics.
10. Set the end-of-month review date.

# Output

Markdown per modules/agency-delivery-kit/monthly-content-plan-template.md. JSON per schemas/monthly-content-plan.schema.json (Mode 2).

End with:
- Quality Checklist
- Missing Inputs / Proof Needed
- Suggested next prompt (typically prompts/04-generate-carousel.md for the first post)

# Constraints

- Don't fill the plan with placeholders ("topic TBD"). Skip if undecided.
- Don't lock rigidly — leave 1-2 slots flexible for opportunistic posts.
- Don't recommend cadences the brand can't maintain.
```

---

## Example invocation

```
Brand: brands/lumen/
Month: 2026-05
Theme: "interrupt-budget thesis push"
Total posts: 18
Channels: linkedin primary, instagram secondary
Active campaigns: 14-day product launch (days 13-26 — Focus Mode v3.4)
Recurring series running: Operator Essays (Wednesdays), Engineering Essays (alt Fridays), Data Drop (first Monday)
Required client inputs already gathered: Mira available for 4 founder posts; Theo available for 2 engineering essays; one anonymized customer case ready.
Off-limits topics: nothing new this month.
```

Claude returns the 30-day plan.

---

## When to use

- Start of each month.
- After a content strategy refresh.
- After a major reposition.

---

## Iterating

```
Drop the 'Data Drop' for May — methodology not cleared yet. Replace with a contrarian post on day 7.
```

```
Move the launch from days 13-26 to days 16-29. Cascade the rest of the schedule.
```
