# Diagnosis Framework

> The method the Brand Diagnosis Agent uses to audit any brand for gaps. Practical, evidence-based, never hand-wavy.

A diagnosis is not "things you could improve." A diagnosis is **specific gaps** between the brand's current state and what would let it compound on social. Vague diagnoses are useless.

---

## The 12 audit categories

For each category: what to check, what gaps look like, what evidence to demand.

### 1. Brand clarity
- Does the brand have a one-liner anyone could repeat?
- Is the category obvious in 5 seconds?
- Does the bio answer "what is this and who is it for"?

**Gap signals:** different one-liners across channels; cluttered bio; jargon-only descriptions.

### 2. Audience clarity
- Is the audience specific (role + situation)?
- Can the brand name 3 things the audience says verbatim?
- Is there a stated *non*-audience?

**Gap signals:** "everyone" framing; generic personas; no quotes from real conversations.

### 3. Positioning
- One-line positioning statement present?
- Differentiated against named alternatives?
- Honest about what it's not?

**Gap signals:** undifferentiated language ("the best X"); positioning vs no-one in particular; refusal to say no.

### 4. Offer clarity
- What does the brand sell, in plain language?
- What's the unique mechanism (not just features)?
- Pricing legible (or strategically opaque on purpose)?

**Gap signals:** offer behind 3 clicks of marketing; mechanism missing; "contact us" with no anchor.

### 5. Trust / proof
- Real customer stories, real numbers, real quotes available?
- Compliance or technical evidence present where category demands?
- Awards, press, public artifacts?

**Gap signals:** no proof at all; only "trusted by [logos]" with no narrative; numbers without methodology.

### 6. Content consistency
- Frequency steady?
- Format mix coherent?
- Series or just one-offs?
- Voice the same across posts?

**Gap signals:** posting cliffs; format chaos; nothing recurring; voice swings between posts.

### 7. Visual consistency
- Recognizable visual signature within 3 posts?
- Typography stable?
- Color palette restrained?
- Templates reused?

**Gap signals:** every post looks like a different brand; new font per slide; 4 accents in one carousel.

### 8. CTA clarity
- Each post has one CTA?
- CTAs match post energy?
- Distinct CTAs across the funnel?

**Gap signals:** "Hope this helps" closings; multi-CTA chaos; single CTA repeated identically across every post.

### 9. Conversion path
- Where does an interested reader go?
- Is it 1 click away?
- Is the destination on-brand?

**Gap signals:** dead-end posts; broken bio links; landing pages that don't match the post's promise.

### 10. Profile / bio clarity
- Bio sentence, link, accent visible?
- First 5 posts on the grid coherent?
- Pinned post is the strongest one?

**Gap signals:** bio reads as biography; link buried; pinned post is 18 months old.

### 11. Authority signals
- Does the brand show *who* makes the decisions?
- Are founders or operators visible?
- Is there a strong public artifact (essay, dataset, OSS, talk)?

**Gap signals:** anonymous corporate voice; no human face; no public artifact.

### 12. Messaging gaps
- Are there obvious topics the brand should own and doesn't?
- Are there topics they post about that aren't theirs?
- Where's the audience having conversations the brand isn't in?

**Gap signals:** the brand never speaks to its strongest pain; competitors own the brand's natural territory.

---

## Scoring

Score each category 1-5:

- **1 — Critical gap.** Active liability.
- **2 — Major gap.** Visible to the audience.
- **3 — Adequate.** Functional but unremarkable.
- **4 — Strong.** Recognizable advantage.
- **5 — Exceptional.** Worth studying.

Compute an overall score (sum / 60).

---

## Output structure

A diagnosis returns:

```
1. Executive summary (3-5 sentences naming the 1-2 biggest unlocks).
2. Category scores (1-5 each, with one-line justification).
3. Problems found (list, ranked by severity).
4. Why each problem matters (one line per problem).
5. Recommended fixes (specific, actionable, ordered by leverage).
6. Quick wins (≤1 week of work, high impact).
7. Strategic opportunities (>1 month of work, high impact).
8. Missing inputs (what we couldn't audit due to insufficient context).
9. Proof needed (claims we'd need to validate to upgrade scores).
10. Next best actions (top 3 things to do this week).
```

See `schemas/brand-diagnosis.schema.json` for the JSON form.

---

## Inputs Claude can work with

The diagnosis does **not** require API access to social platforms. Acceptable input:

- Pasted website copy.
- Pasted bio text.
- Pasted bullet list of recent posts.
- Pasted decks, sales pages, product pages.
- Customer interview transcripts.
- Screenshots described in text.
- An existing partial Brand Pack.

If the input is sparse, Claude scores what it can and flags the rest as `[MISSING_INPUT]`.

---

## What Claude must never do during a diagnosis

- Invent customer names or quote them.
- Fabricate metrics ("Your engagement is 40% below benchmark" — only if you have the benchmark).
- Score positively without evidence.
- Score negatively without specifying the gap.
- Recommend tactics that contradict the brand's stated constraints.
- Use vague language ("more engaging," "better content") — every recommendation must be specific enough to be done by Friday.

---

## Specificity examples

**Bad:** "Improve audience clarity."
**Good:** "Replace the bio's category description ('innovative platform') with a role + situation phrase: 'For senior PMs at 50-500 person SaaS who want their mornings back.'"

**Bad:** "Add more proof."
**Good:** "Slide 6 of the 'state of focus' carousel makes a numerical claim ('40% faster') with no source. Either remove the number or cite a methodology in slide 7. Otherwise the post will be challenged."

**Bad:** "Improve visual consistency."
**Good:** "The last 12 LinkedIn posts use 5 different headline typefaces. Pick one (recommend a serif aligned with the founder voice) and use it on every slide for the next 90 days. Keep the body sans where it currently is."

---

## When to recommend a Brand Pack rebuild

If 4+ categories score 1-2, the diagnosis output should explicitly recommend running `prompts/02-generate-brand-pack.md` before generating new content. Don't fix tactics on top of a broken foundation.

---

## Diagnosis cadence

- New brand → run before any other generation.
- Existing brand → run quarterly, or after any major repositioning.
- After 30 days of posting → run a "30-day audit" version focused on what's been published.

The diagnosis output feeds back into the Brand Pack: low-scoring categories suggest specific fields to refresh in `brands/[slug]/`.
