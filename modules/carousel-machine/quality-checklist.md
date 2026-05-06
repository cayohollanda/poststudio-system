# Quality Checklist — Carousel Machine

> Module-specific items. The full 30-point rubric lives in [`system/quality-checklist.md`](../../system/quality-checklist.md). This file lists the carousel-specific extensions and the most common failure modes.

---

## Module-specific quick check

Beyond the universal 30-point checklist, every carousel must pass these module-specific items:

- [ ] Strategic Angle is 1-3 sentences. Names the angle from `system/content-system.md`. Names the tension.
- [ ] Slide count is 7-10 (default 8).
- [ ] Each slide has all 6 fields (Role, Headline, Support text, Visual concept, Image prompt, Layout instruction). Use `—` for empty.
- [ ] Caption length matches platform sweet spot.
- [ ] CTA matches the angle (see `copywriting-rules.md`'s CTA matching table).
- [ ] First comment adds value (link / bonus / contrarian footnote / receipt) — not caption duplicate.
- [ ] Exactly 5 alternative hooks, each from a different angle/framing.
- [ ] Quality Checklist scorecard at the end.
- [ ] Missing Inputs / Proof Needed at the end.

---

## Top 5 failure modes for this module

1. **Hook is generic.** Listicle openers ("5 things..."), "Are you tired of...?", or no audience-specific entry. → run `prompts/improve-hook.md`.
2. **Slides 2 and 3 argue the same point.** Combine. The carousel should tighten.
3. **No mechanism slide.** Slides 1-3 build pressure; slides 4-5 should release with a *why-this-works* explanation. Without it the post is just complaining.
4. **CTA mismatched to angle.** Founder essay ending with "Try it free!" or workflow tutorial ending with "Save this for later!" Match energy.
5. **Image prompts forgot the negatives.** "no text, no logos, no watermark, no real brand names" must be in every image prompt.

---

## When to bump the slide count

| Signal | Action |
|---|---|
| Topic genuinely thin | drop to 7 |
| Educational with multiple components | bump to 9 |
| Launch with multiple use cases | bump to 10 |
| You're tempted to write 11 | split into a series |

---

## When to drop a slide

If you can't name the slide's *role* in 1 word, the slide doesn't belong. Kill it. Renumber.

---

## When to write a different module

- Topic is short-form video → switch to `modules/reels-script-machine/`.
- Topic is a 7+ post sequence → switch to `modules/campaign-builder/`.
- Topic is "diagnose this brand" → switch to `modules/brand-diagnosis-agent/`.
- Topic is a renderable carousel package → chain into `modules/renderable-creative-system/`.

---

## Reviewer pass

After generating, run `prompts/08-critique-output.md` in a separate context. Don't approve your own draft inline.

For Mode 2 / 3, the CRUD platform makes a separate Claude API call for the critique pass.
