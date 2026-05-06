# Examples — `weaura-ai`

> What's worked, what hasn't, and what we're trying next. The brand has not yet shipped a portfolio of public posts at brand-pack creation; this file scaffolds the capture process.

---

## Reference layout anchors (external)

> Templates from creators we want to **borrow layout patterns from** — never visual identity.

### Anchor 1 — `@brandsdecoded__` (Instagram) → adopted as `decoded-editorial` mode

- **Source:** `@brandsdecoded__` (Brazilian AI Content Agency, 287K followers, "Decodificando o futuro do marketing com AI").
- **Status:** ✅ Template extracted from 3 reference carousels (16 screenshots reviewed on 2026-05-06). Documented in [`visual-style.md`](visual-style.md) as the new primary visual mode `decoded-editorial`.
- **What we borrowed:**
  - Header chrome: thin top accent line + label-left + handle-right
  - Empty space at top of body slides (intentional editorial breathing)
  - Lower-half headline placement (NOT bottom-third)
  - Section eyebrow above headline (uppercase letter-spaced)
  - Body text BELOW headline (NOT above)
  - Slide rhythm alternating light/dark backgrounds
  - Ghost number watermark on dark slides
  - Bottom progress bar + N/total slide indicator
  - Embedded mockups with engagement-pill chips
  - Hero (slide 1) with full-bleed cinematic image + bottom-half bold headline + tease arrow
  - CTA pattern: pill bar + "Comenta a palavra abaixo: [TRIGGER]" card
  - Higher density per slide (80-200 words on body slides)
- **What we did NOT borrow (kept WeAura identity):**
  - Orange accent → VOLT `#CCFF00`
  - "POWERED BY CONTENT MACHINE" footer → "WEAURA" or omitted
  - Druk-style heavy condensed display sans → Manrope Bold (closest brand-locked approximation)
  - Photographic Hero with faces/people → object/blueprint compositions per `constraints.md`
  - "@brandsdecoded__" handle → "@weaura_ai"
  - Brandsdecoded's star/aperture mark → WeAura Aperture mark (real asset, base64-embedded)
- **Trade-offs noted:**
  - Manrope is less compressed than Druk Wide. The headline impact will be slightly softer. If the user wants the exact brandsdecoded heaviness, the next Brand Pack refresh should evaluate adding a real condensed display sans (Druk, Compacta, Knockout) as a secondary headline face. For now, Manrope SemiBold at tight tracking is the closest brand-aligned choice.
  - WeAura's no-faces rule means Hero slides use object photography instead of cinematic portraits. The composition stays similar (full-bleed + dark overlay + bottom-half headline) but the subject differs.
- **Reference assets:** 3 reference carousels are stored locally at `/tmp/brandsdecoded-ref/` for layout consultation. These are **NOT committed** to the repo (copyright). The patterns live in `visual-style.md`'s `decoded-editorial` spec.

---

## What's worked

> _(to be added — capture as posts ship and reactions accumulate.)_

### Worked 1 — _(to be added)_
- **Topic / hook:** _(to be added)_
- **Format:** _(to be added)_
- **Platform:** _(to be added)_
- **Date:** _(to be added)_
- **Why it landed:** _(to be added)_
- **Metric (if available):** _(to be added)_
- **What to repeat:** _(to be added)_

---

## What didn't work

> _(to be added)_

---

## Banned patterns (we tried these, they hurt the brand)

> _(to be added — see `rejected-examples.md` for a starter list of banned patterns inferred from voice / constraints.)_

---

## Rituals (recurring content patterns we've kept)

> Proposed rituals based on the content-pillar plan. Confirm with the team and lock once running.

- **Weekly: "Operator Notes"** — Wednesday or Thursday morning. Senior-engineer-voice carousel on a single recognizable ops pain. Pillar 3.
- **Bi-weekly: "Architecture Notes"** — alternating Fridays. Engineering-essay carousel on a real WeAura architecture decision. Pillars 2 + 4.
- **Monthly: "Grounded Ops" essay** — first Monday of each month. Long-form essay on the thesis (Pillar 1).
- **Quarterly: "We Said No"** — every quarter. Essay on a product / customer / scope decision the team turned down.

---

## Hook patterns the audience responds to

> Inferred from the brand voice + audience profile. Confirm by performance after first 4-6 posts.

- **Italicized-contrast pattern** — "uma IA que adivinha vs uma IA que _sabe_." The brand-signature shape.
- **Concrete pain in 1 sentence** — "It takes 20 minutes to find out why `payment-api` is restarting because the answer lives across 5 dashboards."
- **The reframe pattern** — "AI in ops isn't a chat box. It's the correlation layer."
- **Number-anchored time-savings** — "1.4 seconds. 380 tokens. Source cited." (when proof allows).
- **The "stop X" prescriptive** — "Pare de adivinhar. Saiba." — already a brand signature.

## Hook patterns that don't work for our audience

- **Generic listicles** ("5 AI tools every SRE should try"). Banned.
- **"Hot take:"** openers. Banned.
- **All-caps urgency** ("THE ONE THING..."). Banned.
- **Question-as-hook with no specificity** ("Are you struggling with incidents?"). Banned.
- **Engagement bait** ("Drop a 🔥 if you've ever been paged at 3am"). Banned.

---

## Visual patterns the audience responds to

> Proposed; confirm by performance.

- **Editorial-minimal mode** with cream backgrounds + restrained typography on the bottom-third.
- **Technical-blueprint mode** for engineering-essay slides — thin white linework on deep navy with subtle paper grain.
- **Object-led visual metaphors** (a magnifying glass, a brass dial, a single labeled artifact) for hero slides.
- **Italicized brand-signature words** carried through the slide typography (the underlined-italic verb pattern).

## Visual patterns that don't work

> Inferred from voice + constraints.

- **Cyberpunk / RGB / neon glow.** Off-brand entirely.
- **Glowing brain / Matrix code / abstract dot networks.** "AI brand" clichés the audience scroll-skips.
- **Faces of any kind** (real or AI-generated). Off-brand entirely.
- **Stock-photo composition** (smiling team at laptop). Off-brand.
- **Two-accent gradient backgrounds.** Reads cluttered at thumbnail.

---

## Captions / CTA patterns we want to preserve

> Proposed; confirm by performance.

### Caption opener patterns to test
- **Restate the hook in different words on line 1.** Then the strongest sentence from the carousel on line 2-3.
- **Bilingual restatement** — open in PT, repeat in EN (or vice versa) when both languages add weight.
- **Concrete-pain confession** — "Last week I spent 4 hours pivoting through Grafana, Loki, and ArgoCD just to find out a deploy 3 hours ago was the cause."

### Caption closing / CTA patterns to test
- **One question targeting a specific decision** — "What's the longest RCA you've shipped this quarter?"
- **"Save this for the next time you're about to install another AI tool."** (template adaptable from the system).
- **"30 min com nossa equipe. Você traz um incidente real."** — direct CTA from the public site.

### CTAs that should never ship
- "Thoughts?"
- "What do you think?"
- "Drop a 🔥 if you agree."
- "Tag a friend who needs this."
- "Hope this helps!"

---

## Reference posts (anchors for new generation)

> _(to be added once 3-5 posts have shipped and the team identifies the gold-standard set.)_

For now, the **public site copy itself is the anchor** — particularly:
- The homepage tagline + signature line: *"A IA que conhece sua infraestrutura. Pare de adivinhar. Saiba."*
- The `/context/` "Marina / payment-api" worked example as a tone reference for explaining mechanism.

---

## Series we've run

> _(to be added — none running yet at brand-pack creation. Proposed series listed under Rituals above.)_

---

## Content backlog / inputs

> Topics queued for future posts. The system can pull from here when the team says "give me something to post this week."

- "Why generic AI fails for ops" (Pillar 1).
- "The 5-dashboard pivot tax — measured" (Pillar 3, needs `[PROOF_NEEDED]` survey of the team).
- "What the WeAura citation contract actually checks" (Pillar 2).
- "The pgvector decision — why we didn't pick a dedicated vector DB" (Pillar 4, Theo / engineering voice).
- "Read-only auto-executes; writes need approval. Here's why this is the only safe shape." (Pillar 2).
- "Onboarding a new SRE — the runbook archaeology problem" (Pillar 3).
- "Multi-tenant row-level security with implicit `tenantId`" (Pillar 4, engineering voice).
- "What 'self-hosted, zero data egress' actually means in our deployment" (Pillar 2).
- "The deploy timeline as the most under-used signal" (Pillar 3).

---

## Refresh log

| Date | What changed |
|---|---|
| 2026-05-06 | Initial examples scaffold derived from public site + content-pillar plan. Live performance data to be captured as posts ship. |
