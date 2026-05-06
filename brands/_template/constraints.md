# Constraints — `[brand-slug]`

> What the brand may and may not say, do, or claim. The system enforces this file as a hard boundary.

---

## Claims allowed

> Specific public claims the brand has internally signed off. Each is paired with the supporting evidence (cross-referenced from `proof-assets.md`).

| Claim | Supporting evidence | Notes |
|---|---|---|
| [e.g. "Used by hundreds of teams across 30+ countries"] | [internal usage data, current as of YYYY-MM-DD] | [round to nearest hundred; never say "thousands" without recount] |
| [e.g. "Cuts onboarding time from days to hours"] | [Customer Story 2 in proof-assets.md] | [use only in product-led contexts; do not extend to "minutes"] |
| [e.g. "SOC 2 Type 2 certified"] | [audit report, dated YYYY-MM-DD] | [only state literal certification; do not imply broader compliance] |
| [...] | [...] | [...] |

## Claims forbidden

> Specific claims the brand will **not** make publicly. Each is paired with the reason (legal, ethical, factual, strategic).

| Forbidden claim | Why |
|---|---|
| [e.g. "FDA-approved"] | [we are not FDA-approved; the term implies regulatory status we do not have] |
| [e.g. "Saves you X hours per week"] | [insufficient sample size; range varies too widely to make a single number defensible] |
| [e.g. "AI-powered"] | [generic term; brand voice avoids it; replaced with specific mechanism language] |
| [e.g. "100% accurate"] | [unprovable; sets unrealistic expectation] |
| [e.g. "The leading [category] in [region]"] | [no defensible third-party ranking supports this] |
| [...] | [...] |

---

## Compliance posture

- **Frameworks the brand is held to:** [select from: HIPAA / GDPR / CCPA / FDA / SEC / FINRA / MiFID / PCI-DSS / SOC 2 / ISO 27001 / none]
- **Required disclaimers (per framework):** [list any disclaimer phrases that must appear in captions for posts touching certain topics]
- **Topics requiring legal review before publishing:** [list]

### Compliance examples (fill in only if applicable)

#### Health / wellness
- Required disclaimer: ["This is not medical advice. Consult a healthcare professional."]
- Banned phrases: ["cure," "treat," "diagnose," "guaranteed results"]
- Banned visual patterns: ["before/after photos of bodies/conditions"]

#### Finance / investing
- Required disclaimer: ["This is not financial advice. Past performance is not indicative of future results."]
- Banned phrases: ["guaranteed returns," "risk-free," "double your money"]
- Banned visual patterns: ["return charts without a clear time period and source"]

#### Data / privacy
- Required disclaimer: [if applicable]
- Banned phrases: ["unhackable," "fully anonymous," "zero data"] (unless literally true)

---

## Trigger phrases for legal review

> Phrases that — if they show up in a draft — must trigger a legal/compliance pause before posting.

- "guaranteed"
- "FDA-approved"
- "scientifically proven"
- "the only [category]"
- "never fails"
- "[Real competitor] is wrong / illegal / dangerous"
- [add brand-specific trigger phrases]

When the system encounters one of these in a draft, it should flag it in **Missing Inputs / Proof Needed** with `LEGAL REVIEW: [phrase]`.

---

## Competitor handling

- **Competitors we may name in posts:** [list, or "none"]
- **Competitors we may NOT name:** [list — typically due to legal risk, market sensitivity, or strategy]
- **How to discuss competitors when not naming them:**
  - Acceptable: "the most popular tool in [category]," "the legacy way," "the spreadsheet you've been using."
  - Unacceptable: thinly disguised name-drops, sneer language, or false equivalences.

### Off-limits framings

- [e.g. "We're the [Real Competitor] killer" — never frame this way]
- [e.g. naming a competitor in a side-by-side comparison without authorization]

---

## Image / visual constraints

> Visual choices that are off-limits or require special handling.

- **People in photography:** [allowed / silhouettes only / no people]
- **Faces of real customers:** [yes with signed release / no]
- **Real product UI screenshots:** [yes / no / only with internal sign-off]
- **Real competitor UI screenshots:** [no — never]
- **Brand-adjacent imagery (e.g. partner logos):** [requires written permission]
- **Stock photography:** [allowed / banned / only abstract textures]
- **AI-typical visual artifacts:** [must be removed before shipping; specifically eyes, hands, garbled text]

---

## Topic constraints

> Topics the brand will or won't discuss publicly.

### On-brand topics
- [Topic 1] — green light
- [Topic 2] — green light
- [Topic 3] — green light

### Off-brand topics (never publish)
- [e.g. "internal team conflict"]
- [e.g. "technical specifics that compromise security posture"]
- [e.g. "macro-political commentary"]
- [...]

### Topics requiring case-by-case review
- [Topic — and what determines if it's a go]

---

## Tone constraints

> Tone-level rules beyond what's in `voice.md`.

- **Sarcasm / cynicism level:** [none / light / dry / freely allowed]
- **Self-deprecation:** [allowed / sparingly / no]
- **Profanity:** [no / soft only / freely (rare for B2B)]
- **References to current events:** [allowed if relevant / case-by-case / no]
- **Humor at the expense of customers / users:** [never — including subtle versions]
- **Humor at the expense of competitors:** [never — including via implication]

---

## Refresh log

| Date | What changed |
|---|---|
| [YYYY-MM-DD] | [e.g. "Added 'data residency' as a topic requiring legal review."] |
| [YYYY-MM-DD] | [...] |

---

> When this file is filled in, the system has a clear boundary for what's defensible, what's banned, and what triggers a pause. Vague constraints produce reckless posts.
