# Output Template — Quality Review

> The standard structure of a Quality Gate output. Conforms to `schemas/quality-review.schema.json`.

---

## Markdown form (Mode 1)

```markdown
# Quality Review

**Verdict:** Ship-ready / Ship with flags / Borderline / Rewrite required
**Iteration:** 1
**Content score:** N/30
**Renderability score:** N/10 (when applicable)
**Combined score:** N/40 (when applicable)

## Critical items

| # | Item | Grade | Note |
|---|---|---|---|
| 1 | Slide 1 stops the scroll | pass | ... |
| 2 | Hook earns the swipe | pass | ... |
| 3 | One idea per slide | pass | ... |
| 4 | Tension named and resolved | pass | ... |
| 5 | Mechanism shown, not asserted | pass | ... |
| 6 | Voice matches Brand Pack | pass | ... |
| 7 | No invented claims | pass | ... |
| 8 | Words-to-avoid respected | pass | ... |
| 9 | CTA is one action | pass | ... |
| 10 | Could pass an external reviewer | pass | ... |

## Quality items

[20 rows, same format]

## Renderability items (when applicable)

[10 rows, same format]

## Highest-leverage fix

[One paragraph naming the single change that would most improve the output. Not a list. Not a wishlist. The one thing.]

## Weakest slide and proposed rewrites

**Slide N — [role]**

Current headline: [paste]
Why it's weak: [1-2 sentences]

**Rewrite A** ([angle]): [proposed headline + support]
**Rewrite B** ([angle]): [proposed headline + support]
**Rewrite C** ([angle]): [proposed headline + support]

## Safety / claims violations

[List violations of safety-and-claims-rules.md or constraints.md. If none, write "None detected."]

## Slides to combine or kill

[If two slides argue the same point, name which to combine. If a slide carries no idea, name which to kill. Otherwise "None recommended."]

## Platform fit

[1-2 sentences on whether the output will perform on the specified platform.]

## Missing inputs / Proof needed

- [items]

## Repair recommendations

If the verdict is not Ship-ready:

1. Apply the highest-leverage fix above.
2. Re-run prompts/08-critique-output.md.
3. If iteration 3+, escalate.
```

---

## JSON form (Mode 2 / 3)

```json
{
  "version": "1.0",
  "prompt_version": "v2.0.0",
  "request_id": "<echoed>",
  "generated_at": "2026-05-06T00:00:00Z",
  "review_target": {
    "module": "carousel-machine",
    "brand": "lumen",
    "iteration": 1,
    "subject_request_id": "<original carousel request id>"
  },
  "verdict": "Ship-ready",
  "scores": {
    "content_pass": 28,
    "content_total": 30,
    "render_pass": 10,
    "render_total": 10,
    "combined_pass": 38,
    "combined_total": 40
  },
  "critical_items": {
    "scroll_stops": "pass",
    "hook_earns_swipe": "pass",
    "one_idea_per_slide": "pass",
    "tension_resolved": "pass",
    "mechanism_shown": "pass",
    "voice_matches_brand": "pass",
    "no_invented_claims": "pass",
    "words_to_avoid_respected": "pass",
    "cta_one_action": "pass",
    "passes_external_review": "pass"
  },
  "quality_items": {
    "hook_specific": "pass",
    "no_stale_patterns": "pass",
    "audience_named_in_first_14_words": "pass",
    "slide_count_in_range": "pass",
    "structure_fits_count": "pass",
    "pacing_alternates": "pass",
    "each_slide_answers_previous": "pass",
    "visual_metaphor_present": "pass",
    "no_marketing_jargon": "pass",
    "no_filler_intensifiers": "pass",
    "headlines_in_range": "pass",
    "active_voice": "pass",
    "numbers_concrete_or_proof_needed": "risk",
    "one_visual_mode": "pass",
    "one_accent_color": "pass",
    "headline_in_bottom_third": "pass",
    "margins_consistent": "pass",
    "image_prompts_no_text_no_logos": "pass",
    "caption_has_comment_prompt": "pass",
    "first_comment_value_stacked": "pass"
  },
  "renderability_items": {
    "schema_valid": "pass",
    "canvas_correct": "pass",
    "no_remote_assets": "pass",
    "no_javascript": "pass",
    "no_invented_logos": "pass",
    "fallback_fonts_present": "pass",
    "single_accent": "pass",
    "headline_overflow_estimate": "pass",
    "no_banned_css_or_svg_features": "pass",
    "metadata_complete": "pass"
  },
  "highest_leverage_fix": "Slide 6's proof claim ('we save customers an average of X hours') has no source. Replace with a [PROOF_NEEDED] marker or remove the number entirely. Otherwise the carousel risks a credibility hit at the proof beat — the slide that needs to land hardest.",
  "weakest_slide": {
    "slide_number": 6,
    "current_headline": "We save customers an average of X hours.",
    "why_weak": "Specific number with no proof in proof-assets.md. Reader will discount the rest of the carousel.",
    "rewrites": [
      { "angle": "honest-experiential", "headline": "Our top customers consistently cut [SPECIFIC PROCESS] time.", "support": "[PROOF_NEEDED: cite a real customer story]" },
      { "angle": "mechanism-shift", "headline": "The replacement: one read at 8am. One read at 5pm.", "support": "No chat in between." },
      { "angle": "audience-callout", "headline": "Senior operators don't measure productivity. They measure interrupt budget.", "support": "Same brain, less context-switching." }
    ]
  },
  "safety_violations": [],
  "slides_to_combine_or_kill": [],
  "platform_fit": "Strong fit for LinkedIn — the audience-callout angle and confessional voice align with what saves on this platform. Caption length is in the sweet spot.",
  "missing_inputs": [
    "Slide 6 needs a real before/after metric or a removed claim."
  ],
  "repair_recommendations": [
    "Apply the highest-leverage fix to slide 6.",
    "Re-run prompts/08-critique-output.md as iteration 2.",
    "If iteration 2 still fails the proof claim check, regenerate from a sharper brief."
  ]
}
```

---

## Notes

- The `subject_request_id` cross-references the original generation job — critical for audit trails.
- The `iteration` field tracks repair loops. 1 = first review; 2 = review of first repair; 3+ = escalate.
- `repair_recommendations` is a *short ordered list*, not a wishlist. Aim for 2-3 entries.
- For non-renderable outputs, omit `renderability_items` and the `render_*` score fields.
- For module-specific overrides (Reels script, campaign, brand pack, brand diagnosis), the rubric items shift — see [`critique-rubric.md`](critique-rubric.md).
