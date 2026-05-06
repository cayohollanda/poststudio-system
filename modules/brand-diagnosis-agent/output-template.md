# Output Template — Brand Diagnosis Agent

> The standard structure of a diagnosis output.

---

## Markdown form (Mode 1)

```markdown
# Brand Diagnosis — `<brand-slug>`

**Date:** YYYY-MM-DD
**Inputs reviewed:** [list]
**Overall score:** N/60

## Executive summary

[3-5 sentences. Name the 1-2 biggest unlocks. Don't list every problem; identify what would change the brand's trajectory most.]

## Category scores

| # | Category | Score (1-5) | One-line justification |
|---|---|---|---|
| 1 | Brand clarity | 3 | Bio is clear but tagline is generic. |
| 2 | Audience clarity | 2 | Audience described as "everyone in tech." |
| 3 | Positioning | 2 | No differentiation against named alternatives. |
| 4 | Offer clarity | 4 | Strong product page; mechanism well-explained. |
| 5 | Trust / proof | 1 | No public proof; logos-only on website. |
| 6 | Content consistency | 3 | Posts twice a week; format mix unclear. |
| 7 | Visual consistency | 2 | 5 different headline fonts in last 12 posts. |
| 8 | CTA clarity | 3 | CTAs vary; no single primary action. |
| 9 | Conversion path | 4 | Bio link goes to a coherent landing page. |
| 10 | Profile / bio clarity | 3 | Bio sentence works; pinned post is 14 months old. |
| 11 | Authority signals | 2 | Founders not visible. No public artifact. |
| 12 | Messaging gaps | 2 | Audience hangs out in topic X; brand never posts there. |

**Total: 31/60**

## Problems found (ranked)

1. **[Critical] Trust / proof — Score 1.** No public case studies, no metrics, only logo-wall. Audience cannot validate claims.
2. **[Critical] Audience clarity — Score 2.** "Everyone in tech" framing produces generic content. Specific audience needed.
3. **[Critical] Positioning — Score 2.** Bio reads as one of N similar tools. No differentiator named.
4. **[Major] Authority signals — Score 2.** Founders are absent on social. Brand reads as faceless corporate.
5. **[Major] Visual consistency — Score 2.** Five typefaces across 12 posts. Brand recognition impossible.
6. **[Major] Messaging gaps — Score 2.** The audience's strongest pain is unnamed across 30 days of posts.
7. **[Moderate] Content consistency — Score 3.** No recurring series. Format mix not load-bearing.

## Recommended fixes

### For Problem 1 — Trust / proof
**Fix:** Recruit 2 customers willing to be quoted on a public case study. Capture verbatim quote + outcome + permission flag. Save under `brands/[slug]/proof-assets.md`.
**Effort:** ~1 week (depends on customer responsiveness).
**Leverage:** highest. Without proof, no claim slide will land.

### For Problem 2 — Audience clarity
**Fix:** Replace bio's "everyone in tech" framing with one specific role + situation: "[ROLE] at [SIZE] [STAGE] who [SPECIFIC SITUATION]." Validate with the founder by asking which role's pain they've spent the most hours on.
**Effort:** 30 minutes once the role is chosen.
**Leverage:** unlocks every future post's hook.

[... continue for each problem ...]

## Quick wins (≤1 week)

1. Rewrite bio with a specific audience.
2. Pick one accent color and one headline typeface from `system/visual-framework.md` and apply to next 12 posts.
3. Move the founder onto LinkedIn for one essay this week.

## Strategic opportunities (>1 month)

1. Build a public proof page — 5 customer stories with metrics and methodology.
2. Launch a recurring series (weekly founder essay; bi-weekly engineering essay).
3. Run a brand reposition exercise via `prompts/02-generate-brand-pack.md`.

## Missing inputs

- Customer quotes (you said you have them; you didn't paste them).
- Visual style guide.
- Pricing page copy.

## Proof needed

- Engagement metrics by post format (would let me upgrade the Content Consistency score).
- Customer count + churn rate (would let me upgrade Trust / proof).
- Current ICP definition document (would let me upgrade Audience clarity).

## Next 3 actions (this week)

1. Choose one specific audience role and rewrite the bio.
2. Recruit 2 customers for public case studies.
3. Pick one accent color and lock typography for the next 12 posts.

## Recommended next module

Run [`prompts/02-generate-brand-pack.md`](../../prompts/02-generate-brand-pack.md) to rebuild the foundation. Don't generate new posts on top of this diagnosis until the Brand Pack is refreshed.
```

---

## JSON form (Mode 2)

Conforms to `schemas/brand-diagnosis.schema.json`. Returns the same structure as the Markdown form, with per-category scores and per-problem objects.

```json
{
  "version": "1.0",
  "prompt_version": "v2.0.0",
  "brand_slug": "<slug>",
  "generated_at": "2026-05-06T00:00:00Z",
  "inputs_reviewed": [...],
  "overall_score": 31,
  "overall_total": 60,
  "executive_summary": "...",
  "categories": [
    { "id": "brand_clarity", "score": 3, "justification": "..." },
    { "id": "audience_clarity", "score": 2, "justification": "..." },
    ...
  ],
  "problems": [...],
  "recommended_fixes": [...],
  "quick_wins": [...],
  "strategic_opportunities": [...],
  "missing_inputs": [...],
  "proof_needed": [...],
  "next_actions": [...],
  "recommended_next_module": "brand-pack-builder"
}
```

---

## Notes

- Score honestly. A 4 or 5 is rare unless evidence supports it.
- For each score ≤ 3, the gap must be specific — not "could be improved."
- The `next_actions` block is what the user does Monday. Cap at 3.
- If the diagnosis identifies that the Brand Pack itself is broken, recommend rebuilding it before any tactical fixes.
