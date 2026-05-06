# Prompt — 11 Create Client Delivery

> Packages outputs into a client-ready delivery using the Agency Delivery Kit.

---

## Prompt

```
You are operating PostStudio System.

Package outputs into a client delivery using:
- modules/agency-delivery-kit/handoff-template.md
- modules/agency-delivery-kit/scope-boundaries.md
- modules/agency-delivery-kit/revision-rules.md

# Inputs

- Client / brand: brands/[BRAND_SLUG]/
- Deliverables to include: [list of artifacts: carousel.md, image-prompts.md, caption.md, alternatives.md, render outputs, etc.]
- Cycle context: [month and theme]
- Review SLA: [hours by which client should respond]
- Approval channel: [shared doc / Notion / email / CRUD UI]
- Out-of-scope items the brief tempted us toward: [list — see scope-boundaries.md]
- Internal flags: [PROOF_NEEDED items, Quality Gate flags]

# Process

1. Build the handoff doc per modules/agency-delivery-kit/handoff-template.md:
   - What this is (1-2 sentences).
   - What the client is approving (checklist).
   - SLA.
   - How to respond.
   - What's in the package (file list with links).
   - Flags (proof needed, missing inputs, quality flags).
   - Out-of-scope items (named explicitly).
   - Next step after approval.
2. Add an internal note (do NOT share with client) summarizing:
   - What got fixed in QA.
   - What's flagged.
   - What to watch for in future cycles.
3. Confirm scope boundaries (no quiet inclusions).
4. Confirm revision rules (default 1 round; client can request more per modules/agency-delivery-kit/revision-rules.md).

# Output

A complete handoff doc, ready to send. End with:
- Internal-only "team notes" block clearly delineated (do not share).
- Suggested follow-up cadence (e.g. "next cycle starts on day X").
```

---

## Example invocation

```
Client / brand: brands/lumen/
Deliverables to include: carousel.md, carousel.json, image-prompts.md, caption.md, alternatives.md
Cycle context: May 2026 — interrupt-budget thesis push
Review SLA: 48 hours
Approval channel: shared Notion doc
Out-of-scope items: client asked us to also write the email blast — that's not in this engagement.
Internal flags: Slide 6 has [PROOF_NEEDED] for time-saved metric; Quality Gate critical pass at 28/30.
```

Claude returns the handoff doc.

---

## When to use

- Per deliverable, before sending to client review.
- Per monthly cycle, packaging the cycle's outputs.

---

## When NOT to use

- Internal-only outputs (no client review needed).
- Time-sensitive emergency posts (skip the formal handoff; use a streamlined send).

---

## Customization

Adapt the tone of the handoff to match the engagement:

- Senior-operator client → terse, professional, no fluff.
- Marketing-team client → slightly warmer, more context.
- New engagement (first 3 months) → more explicit explanations.
