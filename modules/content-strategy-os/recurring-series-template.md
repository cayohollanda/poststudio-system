# Recurring Series Template

> A series is a repeating format with a known cadence. Series compound brand recognition because the audience knows what to expect.

---

## Per-series structure

```markdown
## Series — [Name]

**Cadence:** [weekly / bi-weekly / monthly]
**Day:** [day of week, if weekly]
**Format:** [carousel / reel / essay / case study / data drop]
**Anchor structure:** [from system/carousel-framework.md, e.g. "default 8-slide" / "thought-leadership 8-slide" / "benchmark 9-slide"]
**Pillar:** [which content pillar this series anchors to]
**Voice:** [founder name / brand voice / specific operator]
**Length:** [carousel slide count / video duration]
**Visual mode:** [from system/visual-framework.md]
**Default CTA:** [save / comment / follow / DM]
**Active:** [yes / no / paused]
**First episode date:** [YYYY-MM-DD]
**Description:** [one paragraph — what makes this series distinctive, what the audience gets from it, why it's worth running]

**Anti-patterns:**
- [things this series should never do — e.g. "no listicle openers"]

**Episode-naming convention:** [e.g. "Operator Essay #N — [topic]" or "Data Drop: [metric]"]
```

---

## Example (Lumen)

### Series — Operator Essays

**Cadence:** weekly
**Day:** Wednesday
**Format:** carousel
**Anchor structure:** default 8-slide, with `Founder Perspective` or `Mistake` angle
**Pillar:** The interrupt-budget thesis
**Voice:** Mira (founder, first-person)
**Length:** 7-9 slides
**Visual mode:** cinematic-dark (primary) + surreal-product-metaphor (slide 1 hero)
**Default CTA:** save
**Active:** yes
**First episode date:** 2026-02-19
**Description:** Mira's weekly essay on a single decision senior operators face. Confessional voice, mechanism-driven, no product plug until late slides if at all. The series carries the brand's "structural, not behavioral" thesis week after week.

**Anti-patterns:**
- No listicle openers ("5 ways...").
- No product plug before slide 5.
- No multi-CTA endings.

**Episode-naming convention:** "Operator Essay #N — [topic]"

---

### Series — Engineering Essays

**Cadence:** bi-weekly
**Day:** Friday
**Format:** carousel
**Anchor structure:** technical-breakdown 8-10 slide
**Pillar:** Architecture / build-in-public
**Voice:** Theo (CTO, first-person)
**Length:** 8-10 slides
**Visual mode:** technical-blueprint OR cinematic-dark
**Default CTA:** save / DM
**Active:** yes
**First episode date:** 2026-04-15
**Description:** Theo's deep-dive on engineering and architecture decisions. For technical-leadership audiences. Less brand-y than Operator Essays.

---

## Series count rules

- **2 series** — minimum. Two recurring patterns are enough to build expectation.
- **3-4 series** — sweet spot for a scaling brand.
- **5+ series** — only if the team can produce them. Series cliffs damage trust.

---

## When to retire a series

- Performance cliff — saves drop > 50% across 3 consecutive episodes.
- Voice drift — the series stops sounding like the brand.
- Audience asks for something else — listen.
- Maintainer leaves — don't ghost-write a personal series.

When retiring, do it with a final episode. Don't disappear.

---

## Save location

Series definitions live in `brands/[slug]/content-pillars.md` (under each pillar's "Series anchored to this pillar" section), or in a separate `brands/[slug]/series.md` if the brand prefers.
