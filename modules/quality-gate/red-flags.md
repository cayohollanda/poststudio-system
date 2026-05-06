# Red Flags

> Patterns that auto-fail the Quality Gate. No discussion. No "but maybe in this case." Found → fail.

These are absolute. They exist because they cause repeat damage to brands and audiences.

---

## Content red flags

🚩 **Hook starts with "Are you tired of..." or "Did you know that..."**
Stale, scroll-skip cadence.

🚩 **The carousel is mostly text-on-color slides with no visual metaphor.**
Aka glorified PowerPoint.

🚩 **A specific number appears that wasn't in the brief or `proof-assets.md`.**
Fabrication. Auto-fail.

🚩 **The CTA is "Hope this helps!" or "Let me know your thoughts!"**
Wastes the most expensive slide.

🚩 **The post sounds like a corporate blog post.**
Reads as a press release rewritten as a list.

🚩 **The carousel describes a feature without grounding it in a job to be done.**
Brochure energy.

🚩 **Two slides argue the same point with different words.**
Redundancy.

🚩 **The caption is the slides typed out as a paragraph.**
The caption is a separate beat; the audience already swiped.

🚩 **Hashtag soup at the end of the caption.**
Reads as scheduled content marketing.

🚩 **Emoji in every line.**
Voice has no spine.

🚩 **A real competitor named without explicit Brand Pack permission.**
Brand-Pack violation; potential legal exposure.

🚩 **A claim that triggers `constraints.md`'s legal-review list.**
Auto-fail unless flagged for review.

🚩 **The hook is a category hook ("X founders should...") instead of a role + situation hook.**
Specificity gap.

🚩 **Every slide opens with the same word ("Stop. Stop. Stop.").**
Voice tic that reads as AI-output.

🚩 **All-caps emphasis ("THE ONE THING...").**
Spam energy.

🚩 **A claim like "the only," "the best," "industry-leading" without proof.**
Unsupported superiority claim.

🚩 **A "case study" with no real customer named or anonymized.**
Fabricated trust signal.

🚩 **A medical / financial / legal claim without disclaimer.**
Compliance violation.

---

## Renderability red flags (Mode 3)

🚩 **`http://` or `https://` URL in any renderable string.**
Worker will fail or strip.

🚩 **`<script>` tag anywhere.**
Auto-strip; possibly auto-block.

🚩 **`javascript:` URI in `href`.**
Auto-strip.

🚩 **`onclick`, `onload`, or any `on*=` handler.**
Auto-strip.

🚩 **Two accent colors used in one slide.**
Visual mode violation.

🚩 **Real-brand logo as text or image, not provided in `available_assets[]`.**
Fabricated brand asset.

🚩 **`<foreignObject>` in SVG.**
Renderer support uneven; common failure.

🚩 **`position: sticky` in HTML/CSS.**
Headless browser issues.

🚩 **Headline character count > estimated canvas at chosen `font-size`.**
Will overflow at render time.

🚩 **`font-family` chain without a generic family fallback.**
Will fall back to "Times" or system default; brand looks wrong.

🚩 **Slide payload mode mismatch (HTML in svg sub-mode, etc.).**
Schema violation.

🚩 **Template ID not in `available_templates[]`.**
Worker can't resolve.

🚩 **Slot value violates declared type / length / enum.**
Worker validation failure.

🚩 **Inline base64 image > 500KB.**
Bloats output; likely a brand asset that should be referenced by ID.

🚩 **Body dimensions don't match canvas.**
Render will be cropped or padded incorrectly.

---

## Brand Pack red flags

🚩 **Voice file has `_(to be added)_` for `words_to_avoid`.**
Voice can't be enforced.

🚩 **No `proof-assets.md` after 30 days of generating.**
Every claim is `[PROOF_NEEDED]`.

🚩 **Two competing tone of voices specified in different sections.**
Outputs will drift.

🚩 **Visual-style references "the brand's existing assets" without specifying any.**
Ambiguous; renders will guess.

🚩 **`constraints.md` has empty `claims-allowed` and empty `claims-forbidden`.**
No safety boundary.

🚩 **`audience.md` describes "everyone."**
Specificity is dead on arrival.

🚩 **No fictional or real example posts in `examples.md` after first generation cycle.**
System has no anchor.

---

## Process red flags

🚩 **Quality Gate run by the same model in the same context as generation.**
Self-approval bias.

🚩 **A renderable carousel shipped without renderability checks.**
The worker becomes the silent QA.

🚩 **A repair loop > 2 iterations.**
The fix isn't reaching the actual problem.

🚩 **A job missing `prompt_version` metadata.**
Cannot reproduce or debug.

🚩 **A multi-tenant deployment with shared brand context.**
Cross-brand contamination.

🚩 **The CRUD platform rendering its own slides in addition to the worker.**
Boundary violation; dual ownership of asset production.

---

## What to do when a red flag fires

1. Identify the flag.
2. If content red flag → return `verdict: "Rewrite required"` with the flag named in `safety_violations[]` or `critical_fails[]`.
3. If renderability red flag → return `renderability_validated: false`, add to `errors[]`, recommend `prompts/16-repair-renderable-output.md`.
4. If brand-pack red flag → return remediation in the diagnosis or pack-builder output. Don't generate against a broken pack.
5. If process red flag → surface in `warnings[]` or escalate.

The Quality Gate's job is to fail loud. The CRUD platform's job is to listen.
