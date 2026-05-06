# Constraints — `lumen`

> What Lumen may and may not say, do, or claim. Hard boundary for the system.

---

## Claims allowed

| Claim | Supporting evidence | Notes |
|---|---|---|
| "Used by over a thousand operators across 30+ countries" | proof-assets.md → Hard metrics | round to "over a thousand," never inflate to "thousands." |
| "No chat interface, by design" | core product fact, public on lumen.example | central positioning; always allowed. |
| "On-device processing for calendar/inbox where possible; cloud calls scoped and ephemeral" | privacy-architecture page on lumen.example | always cite the page if specifics are needed. |
| "We do not train on your data" | privacy policy on lumen.example | exact phrase only; do not paraphrase to "your data is private." |
| "Median user-reported uninterrupted morning blocks per week: 3 (self-reported, n=147)" | proof-assets.md → Hard metrics | must include "self-reported" and the n. |
| "NPS 62 (Q1 2026, n=312)" | proof-assets.md → Hard metrics | must include the period and the n. |
| "Two protected deep-work blocks per day, auto-defended" | core product feature | always allowed. |
| "The morning brief is read in under 90 seconds" | product spec | always allowed for product-led posts. |

## Claims forbidden

| Forbidden claim | Why |
|---|---|
| "10x your productivity" | unprovable; banned vocabulary; not how we measure value. |
| "AI-powered" | generic; inconsistent with brand voice; replaced with specific mechanism language. |
| "Industry-leading" | no defensible third-party ranking. |
| "Faster than [Real Competitor]" | we do not run head-to-head benchmarks publicly; competitive sensitivity. |
| "Saves you X hours per week" — specific number | sample size insufficient; range too wide to defend a single number. |
| "Guaranteed deep-work" | no guarantees in productivity. |
| "Privacy-guaranteed" | over-claims; replace with specific privacy facts. |
| "All-in-one productivity tool" | contradicts positioning by negation. |
| "Replaces your assistant / chief of staff" | over-claims; demoralizing framing. |
| Quotes from customers we do not have explicit permission to attribute | always banned, even anonymized in cases where the quote could identify a specific customer. |
| "Lumen will [predict / decide / replace] your week" | implies autonomy we do not claim and would not promise. |

---

## Compliance posture

- **Frameworks Lumen is held to:** GDPR (we serve EU users), SOC 2 (Type 1 in 2026; Type 2 in progress, not yet completed).
- **SOC 2 statement to use publicly:** "SOC 2 Type 1 attestation completed (2026); Type 2 in progress." Do not say "SOC 2 certified" without specifying type.
- **Required disclaimers:** none for general posts. For posts touching health/wellness adjacencies (not our core, but occasionally referenced when discussing focus and attention), add a soft footnote in the caption: "This is a productivity tool, not a clinical intervention."
- **Topics requiring legal review before publishing:**
  - Anything implying medical or clinical claims about focus / ADHD / attention.
  - Any direct comparison naming a competitor.
  - Any new metric not already in proof-assets.md.

---

## Trigger phrases for legal review

If a draft contains any of these, the system flags `LEGAL REVIEW: [phrase]` in Missing Inputs:

- "guaranteed"
- "scientifically proven"
- "the only [category]"
- "never fails"
- "100%" (in any productivity-related claim)
- "treats" / "cures" (when discussing focus / attention / ADHD)
- "FDA" (any reference)
- "Reclaim is [adjective]" / "Clockwise is [adjective]" / any direct competitor adjective
- "best AI tool" / "best productivity tool"
- "outperforms"

---

## Competitor handling

- **Competitors we may name in posts:** none directly. We may name a *category* of tool ("calendar-AI," "general-purpose chat assistants," "second-brain apps") but not a specific brand.
- **Competitors we may NOT name:**
  - Reclaim, Clockwise, Motion, Akiflow, Sunsama (all calendar-AI / focus-tool space).
  - General-purpose chat assistants (we describe by category, not by name).
  - Second-brain apps (Notion, Obsidian, Roam) — we describe by category.
- **How to discuss competitors when not naming them:**
  - Acceptable: "calendar-AI tools," "general-purpose AI chat tools," "second-brain stacks," "the spreadsheet you've been using."
  - Acceptable: "the most common alternative is..." (without naming).
  - Unacceptable: thinly disguised name-drops, sneer language, false equivalences, or implying named competitors are inferior.

### Off-limits framings

- "We're the [Real Competitor] killer." → never.
- Side-by-side comparison tables naming competitors. → never.
- "[Competitor] users are switching to Lumen." → never (no source).

---

## Image / visual constraints

- **People in photography:** no people, no faces, no hands. Anonymity is part of the brand.
- **Faces of real customers:** no — never publish a customer face.
- **Real product UI screenshots:** allowed only if the data shown is sample/redacted. Never real-customer data.
- **Real competitor UI screenshots:** never.
- **Brand-adjacent imagery (e.g. partner logos):** none. We do not show partner logos.
- **Stock photography:** banned. We render or commission.
- **AI-typical visual artifacts:** must be removed or hidden before shipping (eyes, hands, garbled text). Easy in our case because no people.

---

## Topic constraints

### On-brand topics (green light)
- Interrupt budget, decision load, attention economics.
- Senior-operator workflows, founder operator stories.
- The structural fix for reactive weeks.
- The case against chat-shaped AI for focus work.
- Privacy-respecting AI architecture.
- Mira's and Theo's first-person operator essays.

### Off-brand topics (never publish)
- Macro-political commentary. (Never.)
- Funding / valuation gossip about Lumen or competitors.
- "Hot takes" about other founders / public figures.
- Productivity hacks that contradict the structural-not-behavioral thesis ("just be more disciplined").
- Anything that turns the audience into the problem ("you're lazy").

### Topics requiring case-by-case review
- ADHD / neurodivergence (we are not a clinical product; framing must be careful).
- Workplace surveillance debates (we have a position; we publish only in long-form essays, not carousels).
- Any commentary on news events (case-by-case; default no).

---

## Tone constraints

- **Sarcasm / cynicism level:** dry, sparing. Never directed at users, customers, or named competitors. Sarcasm about *the category* (chat-shaped AI) is allowed.
- **Self-deprecation:** allowed when Mira or Theo is the subject. ("I installed fourteen AI tools in a quarter. My deep-work time was cut in half.") Never about Lumen-the-product as a self-deprecating joke.
- **Profanity:** none.
- **References to current events:** only when directly relevant to the audience's work (e.g. an industry-wide AI shift). Never political, cultural, or trending hashtag bait.
- **Humor at the expense of customers / users:** never, including subtle versions.
- **Humor at the expense of competitors:** never. The category yes; specific brands no.

---

## Refresh log

| Date | What changed |
|---|---|
| 2026-04-30 | Added "Q1 2026, n=312" requirement when citing NPS. Added trigger phrase "treats" / "cures." |
| 2026-03-15 | Added on-device processing claim language (locked exact phrasing). |
| 2026-02-01 | Initial Brand Pack import. |
