# Proof Copy — Pattern Library

> Patterns for citing real proof. Every pattern requires real evidence in `proof-assets.md`. Mark gaps with `[PROOF_NEEDED]`.

---

## Anonymized customer proof

```
A [SEGMENT_DESCRIPTION] cut [PROCESS] from [BEFORE_TIME] to [AFTER_TIME].
```

Example: "A VP of Engineering at a Series-C devtools company cut their daily-decision time from 90 minutes to under 20."

```
[ANONYMIZED_ROLE_AND_COMPANY]: "[VERBATIM_QUOTE]"
```

Example: `VP of Engineering, Series-C devtools: "The brief stopped me from re-deciding the day at every Slack ping."`

---

## Metric proof

```
[METRIC] (self-reported, n=[N]) — as of [DATE].
```

Example: "Median 3 uninterrupted morning blocks per week (self-reported, n=147) — as of 2026-04-30."

```
NPS [SCORE] (Q[N] [YEAR], n=[N]).
```

Always include n and the period.

---

## Public artifact proof

```
[ARTIFACT] is public at [URL/PATH]. [WHAT_IT_SHOWS].
```

Example: "Our privacy architecture is public at lumen.example/privacy-architecture. It shows how on-device processing handles calendar/inbox parsing."

---

## Founder anecdote proof

```
[FOUNDER_NAME, ROLE] on [TOPIC]: "[VERBATIM_ANECDOTE]"
```

Always verbatim. Don't paraphrase.

---

## Press / mention proof

```
[PUBLICATION] covered [WHAT] in [DATE]. [URL].
```

Only with permission and accuracy. Don't imply press you don't have.

---

## "Available but can't cite publicly"

If proof exists but isn't citable, surface it without citing:

```
We've seen this pattern across [SEGMENT_DESCRIPTION]. Specific numbers stay private at customer request.
```

This is honest. Don't fake numbers to fill the gap.

---

## When proof is missing

Mark the slide explicitly:

```
- Headline: We cut [PROCESS] from [BEFORE_TIME] to [AFTER_TIME]
- Support text: [PROOF_NEEDED: real before/after numbers from a real customer]
```

And surface in `Missing Inputs / Proof Needed`:

```
- Slide 6 needs real before/after time numbers.
```

---

## Banned proof patterns

- "Used by thousands of [AUDIENCE]" — not citable without source.
- "Most of our customers see [OUTCOME]" — what's "most"?
- "Industry-leading [METRIC]" — versus whom? cite or remove.
- "Trusted by Fortune 500" — only if you have signed permission to say it.
- Quotes attributed to "a customer" with no identifying segment.
- Generic case-study energy ("Acme Co saved $X in Y months") without the real metric source.

---

## When NOT to use proof copy

- Hook slide. Proof on slide 1 is too early; the audience hasn't earned it.
- Reframe slide. The reframe is opinion + mechanism; proof comes after.
- CTA slide. Proof here weakens the CTA.

Proof lives in slides 5-7 typically (or its own dedicated benchmark / case-study post).
