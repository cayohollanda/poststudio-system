# Critique Rubric

> The grading rubric used by the Quality Gate. 10 critical items + 20 quality items + 10 renderability items (when applicable).

---

## Critical items (10) — any fail = no ship

| # | Item | Pass test |
|---|---|---|
| 1 | Slide 1 stops the scroll | Read alone: would a tired person on Sunday night swipe? |
| 2 | Hook earns the swipe | Slide 2 must be unavoidable after slide 1 |
| 3 | One idea per slide | Each slide's idea writeable in ≤4 words |
| 4 | Tension named and resolved | Fault line is identifiable; release is on slide 4-5 |
| 5 | Mechanism shown, not asserted | At least one slide explains *why* the new approach works |
| 6 | Voice matches Brand Pack | Re-read against `brands/[slug]/voice.md` — would Mira (or this brand's voice owner) say this? |
| 7 | No invented claims | Every number, customer, award, quote sourced or `[PROOF_NEEDED]` |
| 8 | Words-to-avoid respected | No banned vocabulary from `brands/[slug]/voice.md` or system bans |
| 9 | CTA is one action | Not "save and share and follow" — one specific ask |
| 10 | Could pass an external reviewer | Confident yes — no soft spot you'd hide |

---

## Quality items (20) — each fail = flag, not block

| # | Item | Pass test |
|---|---|---|
| 11 | Hook is specific | Audience-named, situation-named, or number-anchored |
| 12 | No stale patterns | No "Are you tired of...?" / "Did you know...?" / "Hot take:" |
| 13 | Audience named in first 14 words | Slide 1 implies or names the segment |
| 14 | Slide count in range | 7-10 (default 8); launch up to 10; benchmark up to 9 |
| 15 | Structure fits count | Default / launch / educational / etc. matches the count |
| 16 | Pacing alternates pinch/release | No 2 low-energy slides back-to-back |
| 17 | Each slide answers previous | Slide 4 answers slide 3's question |
| 18 | Visual metaphor present | At least 1-2 slides use metaphor, not text-on-rectangle |
| 19 | No marketing jargon | No "synergy," "transform," "supercharge," "unleash," etc. |
| 20 | No filler intensifiers | No "absolutely," "literally," "100%" |
| 21 | Headlines 6-12 words | Anything longer becomes blog copy |
| 22 | Active voice | Headlines and slide copy in active voice |
| 23 | Numbers concrete or `[PROOF_NEEDED]` | No vague "thousands of users" without source |
| 24 | One visual mode | Same mode across all slides |
| 25 | One accent color | One per slide, one across the carousel |
| 26 | Headline placement consistent | Bottom-third by default; same per slide |
| 27 | Margins consistent | 48px (or brand override) on every slide |
| 28 | Image prompts say "no text, no logos" | Every image prompt explicitly excludes text artifacts |
| 29 | Caption has comment prompt | Closing line invites a specific reply, not "Thoughts?" |
| 30 | First comment is value-stacked | Adds value (link, bonus, footnote, receipt) — not caption duplicate |

---

## Renderability items (10) — Mode 3 only; any critical fail blocks render

| # | Item | Pass test |
|---|---|---|
| R1 | Schema valid | JSON conforms to `schemas/renderable-carousel.schema.json` |
| R2 | Canvas correct | Matches request (default 1080×1350) |
| R3 | No remote assets | No `http://`, `https://` URLs |
| R4 | No JavaScript | No `<script>`, `on*=`, `javascript:` |
| R5 | No invented logos | No fake brand marks not in `available_assets[]` |
| R6 | Fallback fonts | Every text element ends in a generic font family |
| R7 | Single accent applied selectively | Per slide, ≤ 1 element with accent |
| R8 | No headline overflow | Char count fits canvas at chosen font-size |
| R9 | No banned CSS / SVG features | No sticky, no backdrop-filter, no foreignObject, no viewport units |
| R10 | Metadata complete | Each slide has brand_slug, role, accent_color, headline in metadata |

---

## Module-specific overrides

### Reels script
- Replace items 14-15 (slide count) with: word count fits duration; beats fit duration.
- Replace item 18 (visual metaphor) with: each beat has visual + audio + on-screen direction.
- Replace item 26 (headline placement) with: hook in first 2s, on-screen text legible sound-off.

### Campaign
- Replace items 1-2 (slide-level hook) with: campaign opener post stops the scroll; audience journey is mapped.
- Add: each post has a distinct role in the 5-act arc; CTA path coherent.

### Brand pack
- Replace content rubric with: every section filled or `[MISSING_INPUT]`; voice file has banlists; visual style references the universal framework; constraints lists trigger phrases.

### Brand diagnosis
- Replace content rubric with: 12 categories scored 1-5 with one-line justification; recommendations are specific (doable by Friday); missing inputs surfaced.

---

## How to grade

For each item, mark `pass / fail / risk`:

- **pass** — clearly meets the bar.
- **fail** — clearly does not meet the bar; rewrite needed.
- **risk** — borderline; ship only with explicit acceptance from the consumer.

Be honest. Inflated scores defeat the purpose.

---

## Scoring rule

```
score = (pass count across all 30 items, +10 if renderable)
total = 30 (or 40 with renderability)
```

| Score | Verdict |
|---|---|
| ≥ 27/30 | Ship-ready |
| 24-26/30 | Ship with flags |
| 22-23/30 | Borderline; consumer decides |
| < 22/30 | Rewrite required |
| Any critical fail | Rewrite required regardless of score |

For renderability:

| Renderability score | Action |
|---|---|
| 10/10 | Dispatch render job |
| 7-9/10 with no critical fails | Dispatch with warnings |
| Any critical fail | Repair or block render |

---

## What the critique returns

See [`output-template.md`](output-template.md) for the full output shape.
