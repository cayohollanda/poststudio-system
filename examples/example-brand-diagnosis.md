# Example Brand Diagnosis — `lumen` (fictional)

> Worked example. Shows what a strong diagnosis output looks like. Lumen is fictional — see [`examples/example-brand-pack/`](example-brand-pack/).

**Date:** 2026-04-22
**Inputs reviewed:** bio, last 12 LinkedIn posts, founder essay, customer interview transcripts × 3, product page copy.
**Overall score:** 38/60

---

## Executive summary

Lumen has strong product clarity and a defensible visual identity, but is leaving 30%+ of its potential audience growth on the table by under-investing in two areas: founder visibility (Mira's voice appears on only 30% of posts despite being the brand's strongest asset) and proof artifacts (no public case studies after 8 months of customers). Fixing these two will compound. Everything else is adequate or better.

---

## Category scores

| # | Category | Score | One-line justification |
|---|---|---|---|
| 1 | Brand clarity | 4 | Bio is plain-language; tagline ("build the morning. own the day.") is recognizable. |
| 2 | Audience clarity | 4 | "Senior PMs and engineering leads at 50-500 person SaaS" is specific; non-audience explicit. |
| 3 | Positioning | 5 | "No chat. By design." is a defensible category-defining position. |
| 4 | Offer clarity | 4 | Mechanism (interrupt-budget) explained on the home page; pricing legible. |
| 5 | Trust / proof | 2 | No public case studies. Quotes anonymized but no narrative. NPS cited without n until recently. |
| 6 | Content consistency | 3 | Operator Essays weekly cadence holds; Engineering Essays slipping (6/10 weeks). |
| 7 | Visual consistency | 4 | Cinematic-dark mode + amber accent + GT Sectra typography lock is strong. |
| 8 | CTA clarity | 3 | Save and DM are mixed; no single primary CTA across the funnel. |
| 9 | Conversion path | 4 | Bio link → coherent landing page with mechanism explanation. |
| 10 | Profile / bio clarity | 4 | Bio sentence is sharp; pinned post is recent. |
| 11 | Authority signals | 2 | Mira appears on 30% of posts. Theo appears on 15%. No public artifact (essay collection / talk / whitepaper). |
| 12 | Messaging gaps | 3 | Audience pain "interrupt budget" is named; pain "decision fatigue" is unaddressed despite recurring in interviews. |

**Total: 38/60**

---

## Problems found (ranked)

1. **[Critical] Trust / proof — Score 2.** No public case studies after 8 months. Audience cannot validate the structural-not-behavioral claim with real receipts. Every proof slide currently relies on anonymized quotes; readers discount.
2. **[Critical] Authority signals — Score 2.** Founders are present in the Brand Pack but absent in 70% of posts. The brand reads as faceless when its strongest asset is two named operators.
3. **[Major] Messaging gaps — Score 3.** "Decision fatigue" surfaces in 7/12 customer interviews but appears in 0 of last 12 posts. Audience pain unmet.
4. **[Moderate] Content consistency — Score 3.** Engineering Essays cadence slipping (6/10 weeks in last 10). Trust erodes when a series goes from weekly to "sometimes."

---

## Recommended fixes

### For Problem 1 — Trust / proof
**Fix:** Recruit 2 customers willing to be quoted on a public case study within 4 weeks. Ideal candidates: VP-Engineering at devtools company (already in `proof-assets.md` Story 1) and CEO/founder at fintech (Story 2). Ask each: 1 quote, 1 metric, 1 paragraph of context. Save in `brands/lumen/proof-assets.md` with `permission_to_cite_publicly: yes`.

**Effort:** ~2 weeks (depends on customer responsiveness).
**Leverage:** highest. Without proof, no claim slide will land.

### For Problem 2 — Authority signals
**Fix:** Increase Mira's voice to 50% of posts via the Operator Essays series (currently weekly; keep weekly but assign Mira as primary voice). Add Theo's Engineering Essays back to bi-weekly (was slipping). Publish one quarterly artifact: an essay collection PDF with the 8 best Mira essays, branded.

**Effort:** small per post; ~4 hours for the artifact.
**Leverage:** high. Audience growth on LinkedIn correlates with founder visibility.

### For Problem 3 — Messaging gaps
**Fix:** Plan one Operator Essay specifically on decision fatigue (use audience interviews verbatim). Frame as: "operators don't lose energy from work; they lose it from re-deciding the same thing." Place in next 30-day plan.

**Effort:** standard carousel.
**Leverage:** mid-high. Adds a new pillar topic that maps to existing pain.

### For Problem 4 — Content consistency
**Fix:** Lock Engineering Essays cadence by pre-drafting 4 Theo posts a month ahead. If Theo unavailable, ghost-write under Mira's voice with explicit "co-authored" tagging. Don't break the cadence.

**Effort:** small; pre-batching.
**Leverage:** mid.

---

## Quick wins (≤1 week)

1. Schedule customer interviews for 2 case studies. Send today.
2. Re-anchor Engineering Essays — Theo to commit 4 posts in next 4 weeks.
3. Draft the decision-fatigue Operator Essay for next Wednesday.

---

## Strategic opportunities (>1 month)

1. Publish a quarterly essay collection (PDF + landing page).
2. Build a public proof page with 5 customer stories with metrics and methodology.
3. Run a 30-day decision-fatigue authority campaign in Q3.

---

## Missing inputs

- Engagement metrics by post format (would let me upgrade Content Consistency score).
- Customer count + churn rate (would let me upgrade Trust / proof if the data is citable).
- Founder availability mapping (so we can plan founder voice cadence accurately).

---

## Proof needed

- Customer A's permission to use specific time-saved metric publicly.
- Customer B's permission to be named (currently anonymized; named would unlock authority).
- NPS methodology document (we cite "n=312, Q1 2026"; we should also have the methodology cite-able).

---

## Top 3 actions (this week)

1. Send customer interview requests to Story 1 + Story 2 customers for public case studies.
2. Theo commits to 4 Engineering Essays in next 4 weeks (calendar-blocked).
3. Decision-fatigue Operator Essay drafted for next Wednesday's slot.

---

## Recommended next module

[`prompts/03-generate-content-strategy.md`](../prompts/03-generate-content-strategy.md) — refresh the next 30-day plan with the decision-fatigue topic, increased Mira cadence, and a planned proof drop in week 4.

No Brand Pack rebuild needed; the foundation is strong.
