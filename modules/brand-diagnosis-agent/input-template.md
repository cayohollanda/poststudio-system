# Input Template — Brand Diagnosis Agent

> What to provide so the diagnosis is useful. Sparse inputs produce sparse diagnoses.

---

## Minimum viable input

At least one of:

- Brand website URL (for human reference; do not visit) + pasted homepage copy.
- Bio + recent post titles or first lines.
- Existing partial Brand Pack at `brands/[slug]/`.

If none of the above are available, the diagnosis cannot start. Use the Brand Pack Builder first.

---

## Recommended input

For a strong diagnosis, paste:

### 1. Identity layer
- Brand name + slug.
- Bio / about page text.
- Tagline.
- Founders / public-facing voices.

### 2. Audience layer
- Pasted copy describing the audience as the brand currently describes it.
- 2-3 quotes from real customers (interview transcripts, reviews, DMs).
- Where the brand currently posts.

### 3. Offer layer
- Product page copy (homepage, pricing, features).
- Mechanism explanation (how does it work?).
- Pricing tiers (if public).

### 4. Voice layer
- 3-5 example posts (paste the copy, not screenshots).
- 2-3 founder essays / talks / interviews if available.

### 5. Visual layer
- Description of the current visual style (or 3-5 example post screenshots described in text).
- Color palette (hex if known).
- Typography (if known).

### 6. Proof layer
- Public case studies.
- Testimonials.
- Metrics they cite publicly.

### 7. Constraint layer
- Any compliance posture (HIPAA / GDPR / SEC / SOC 2 / etc.).
- Any topics off-limits.
- Any competitors off-limits to name.

### 8. Posting cadence
- How often they post per channel.
- Whether they have recurring series.

---

## Input format

Paste each section in order, headed by the section name. For example:

```
=== Bio ===
[paste]

=== Recent posts (last 10) ===
1. [title or first line]
2. ...

=== Founder essays ===
[paste 1-3]

=== Customer quotes ===
[paste 2-3]

[etc.]
```

Or share the existing partial Brand Pack and the agent will read it.

---

## What you should *not* include

- Sensitive customer data without consent.
- Internal financials.
- Confidential strategy documents.
- Real names or quotes of customers who haven't agreed to public use.

The diagnosis treats all input as if it could be discussed publicly. Don't paste what you wouldn't want repeated.

---

## Sparse-input behavior

If you provide partial input, the diagnosis:

- Scores what it can.
- Marks missing categories as `_(unscorable: missing input X)_`.
- Lists `Missing inputs` at the end.
- Recommends what to provide for a follow-up audit.

Don't expect a full 12-category diagnosis from a one-sentence brief.
