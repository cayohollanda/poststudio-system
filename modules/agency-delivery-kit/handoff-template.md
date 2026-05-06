# Handoff Template

> The standard delivery package sent to the client for review or for final ship. Same shape every time.

---

## What's in the handoff

```markdown
# [Brand] — [Deliverable type] — [Date]

## What this is

[1-2 sentences. The brief in plain language.]

## What you're approving

- [ ] [Deliverable 1 — e.g. carousel, 8 slides]
- [ ] [Deliverable 2 — e.g. caption + first comment]
- [ ] [Deliverable 3 — e.g. CTA + alt hooks]

## SLA

[We need your feedback by YYYY-MM-DD HH:MM to ship by YYYY-MM-DD.]

## How to respond

- For approvals: reply "approved" or check the boxes above.
- For changes: use the inline comments OR reply with the requested change.
- For questions: ask any. We'll respond within [agreed SLA].

## What's in this package

[link or attached files]

- `carousel.md` — slide-by-slide content
- `image-prompts.md` — refined image prompts per slide
- `caption.md` — caption + CTA + first comment
- `alternatives.md` — 5 alt hooks for testing
- `notes.md` — internal notes (what's flagged, what's missing)

## Flags

- [list any [PROOF_NEEDED], [MISSING_INPUT], or quality flags]

## Out of scope

[If the brief was tempted to expand, name what we did NOT do. Reference scope-boundaries.md.]

## Next step after approval

[What happens once the client approves. Render? Schedule? Hand to designer?]

## Internal note (do not share with client)

[Private context for the team — what got fixed, what's flagged, what to watch for in future cycles.]
```

---

## File naming

Each handoff lives at:

```
outputs/[client-slug]/[YYYY-MM-DD]-[topic-slug]/
  review/
    HANDOFF.md          # this template, filled in
    carousel.md
    image-prompts.md
    caption.md
    alternatives.md
    notes.md
```

When approved, copy to `final/` with timestamp.

---

## Tone of the handoff

- Professional, not corporate.
- Confident in what's being delivered.
- Honest about what's flagged.
- Specific about what we need from the client (and by when).

The client should be able to read the handoff in 5 minutes and know exactly what to do next.

---

## What NOT to put in the handoff

- "We hope you love it!"
- "Let us know if anything needs to change!"
- Vague status ("almost done").
- Surprises (a feature deck the brief didn't ask for).
- Internal-only notes mixed with client-facing copy.
