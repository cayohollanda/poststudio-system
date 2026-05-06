# Rejected Examples — `weaura-ai`

> Patterns banned because they hurt the brand. WeAura has not yet shipped enough public posts to have a long banned-pattern list; entries below are seeded from voice / constraints / category hygiene.

---

## Banned patterns (seeded — no real-world examples yet)

### Pattern: Generic listicle hook ("5 AI tools every SRE should try")
- **Description:** numbered-list hooks that could come from any AI brand.
- **Why it would fail:** generic; no audience-specific framing; no mechanism. The audience equates listicles with low-effort content marketing.
- **What to do instead:** italicized-contrast hook ("uma IA que adivinha vs uma IA que _sabe_") OR concrete-pain confession ("Last week I spent 4 hours pivoting between Grafana, Loki, and ArgoCD just to find out a deploy 3 hours ago was the cause.").
- **Date banned:** 2026-05-06 (preemptive, not reactive — banned before first post).
- **Banned by:** brand-pack creation per voice rules.

### Pattern: Direct competitor comparison by name
- **Description:** naming a specific AI ops tool, observability vendor, or generic AI assistant in a side-by-side comparison.
- **Why it would fail:** reads adversarial; risks misrepresenting the competitor; compliance flag per `constraints.md`.
- **What to do instead:** compete on category positioning ("generic AI chat," "observability AI features," "internal RAG-over-Confluence projects").
- **Date banned:** 2026-05-06.
- **Banned by:** `constraints.md` rule.

### Pattern: All-caps emphasis or hype intensifiers
- **Description:** "THE ONE THING SREs are missing" / "INSANE results" / "10x faster RCA" / "supercharge your ops."
- **Why it would fail:** every banned word in the voice file. Reads as low-effort marketing copy. Audience scroll-skips.
- **What to do instead:** restrained emphasis through italicized verbs ("_conhece_" / "_knows_") or accent color on a single word in slide typography.
- **Date banned:** 2026-05-06.

### Pattern: Three-line poetry caption layout
- **Description:** "this. is. a. line." caption formatting where each phrase is a separate line.
- **Why it would fail:** contradicts the restrained, technical voice. Reads as theatrical / LinkedIn-bro.
- **What to do instead:** conversational paragraphs with one longer sentence per paragraph for rhythm.
- **Date banned:** 2026-05-06.

### Pattern: "AI-powered" generic descriptor
- **Description:** describing WeAura as "AI-powered" without specifying the mechanism.
- **Why it would fail:** generic — every brand says it. The audience equates "AI-powered" with marketing fog.
- **What to do instead:** specify the mechanism: "grounded in your infrastructure," "with citation contract," "with HITL approval," "with self-hosted zero data egress."
- **Date banned:** 2026-05-06 (per `voice.md` brand-specific banlist).

### Pattern: Mid-post product plug
- **Description:** mentioning WeAura by name in the middle of an Operator Notes / engineering essay.
- **Why it would fail:** breaks trust. Audience reaction is "this turned into an ad mid-essay."
- **What to do instead:** reserve product mentions for slide 5+ in product-led structure only. In Operator Notes / engineering essays, no product mention until the final slide (or none at all if the post stands alone as thought leadership).
- **Date banned:** 2026-05-06 (preemptive per general system best-practice; same rule the Lumen example brand applied).

### Pattern: Cyberpunk / RGB / "AI brand" visual clichés
- **Description:** glowing brains, Matrix-style code rain, abstract networks of dots, neon RGB cyber aesthetics.
- **Why it would fail:** wrong category for the audience. Senior engineers scroll past these as marketing-team-without-taste signals.
- **What to do instead:** editorial-minimal (cream backgrounds, object metaphors) or technical-blueprint (thin linework on deep navy). See `visual-style.md`.
- **Date banned:** 2026-05-06 (per `visual-style.md` do-nots).

### Pattern: Engagement-bait CTAs
- **Description:** "Drop a 🔥 if you've ever been paged at 3am" / "Tag a friend who needs this" / emoji-reaction prompts.
- **Why it would fail:** off-brand for the technical audience. Reads as scheduled content marketing.
- **What to do instead:** specific question CTA ("What's the longest RCA you've shipped this quarter?") OR save-trigger ("Save this for the next time you're about to install another AI tool.") OR demo CTA ("30 min com nossa equipe. Você traz um incidente real.").
- **Date banned:** 2026-05-06.

---

## Common categories of banned patterns

### Hook patterns (banned)
- Listicle hooks ("5 ways..." / "7 things every SRE...").
- "Are you tired of...?"
- "Did you know that...?"
- "Hot take:" / "Unpopular opinion:"
- "Buckle up." / "Strap in."
- All-caps hooks.
- Engagement bait.

### Visual patterns (banned)
- Bright, oversaturated tech aesthetic.
- Glowing brains / Matrix code / abstract dot networks.
- Faces (real or AI-generated).
- Stock photography of teams at laptops.
- Two-accent gradient backgrounds.
- Real competitor UI screenshots.

### Caption / CTA patterns (banned)
- "Hope this helps!"
- "Let me know your thoughts."
- "Thoughts?"
- "Tag a friend who needs this."
- Hashtag soup at the end (1-2 hashtags max, brand-pack-approved only).
- Emoji-reaction prompts.

### Topic / angle patterns (banned)
- AI-doomer / AI-utopian content not connected to grounded ops.
- Funding / valuation gossip.
- Macro-political commentary.
- Customer-vertical name-drops without permission.

---

## Banned for compliance reasons

> Distinct from "tried and failed." These patterns are banned because they violate `constraints.md` or trigger legal review.

- "FDA-approved" / "HIPAA-compliant" / "PCI-DSS-certified" / "FedRAMP" — only if the certification is added to `proof-assets.md`.
- "Auto-remediation" / "self-healing" / "autonomously fixes" — never. WeAura explicitly does not auto-execute writes.
- "Zero hallucinations" — over-claim. Replace with the citation-contract facts.
- "[Specific Real Competitor] is slow / unsafe / wrong" — never.
- "Replaces your SREs" — never. Off-brand and over-claim.
- Specific health-outcome / financial-outcome claims about customer industries — never without customer permission and disclaimer review.

---

## Patterns we tried that almost worked

> _(to be added — none yet at brand-pack creation. Capture as posts ship and patterns emerge.)_

---

## Refresh cadence

- Quarterly review of all banned patterns. Some may be unbanned if circumstances change.
- Add new bans whenever a deliverable underperforms in a way the team agrees on.
- Cross-reference monthly reports for new candidates.

---

## How the system uses this file

When generating any output for `weaura-ai`, Claude:

1. Loads this file alongside `voice.md` and `constraints.md`.
2. Treats banned patterns as hard constraints.
3. Surfaces flagged conflicts in `Missing Inputs / Proof Needed` if a brief tempts a banned pattern.

This file compounds. The longer WeAura operates, the sharper this file becomes — and the better future content gets.

---

## Refresh log

| Date | What changed |
|---|---|
| 2026-05-06 | Initial banned-pattern list seeded from voice / constraints / category hygiene. Real-world performance data to be captured as posts ship. |
