# Playbook — Client Delivery Workflow

> The agency-side production loop. Per deliverable, per client. Mode 1.

The full implementation lives in [`modules/agency-delivery-kit/`](../modules/agency-delivery-kit/). This playbook is the day-to-day operational checklist.

---

## Pre-deliverable

- [ ] Brand Pack at `brands/[client-slug]/` is current.
- [ ] Monthly content plan exists; today's deliverable is in it.
- [ ] Required inputs collected (per `input-collection-checklist.md`).
- [ ] Output folder created: `outputs/[client-slug]/[YYYY-MM-DD]-[topic-slug]/`.

---

## Stage 1 — Brief

Pull from the monthly plan. Refine if needed.

```
Topic:    [...]
Audience: [...]
Goal:     [...]
Signal:   [...]
Format:   [carousel | reel | campaign | data drop]
```

Save as `outputs/[client-slug]/.../brief.md`.

---

## Stage 2 — Generate

Run the appropriate prompt:

- Carousel → `prompts/04-generate-carousel.md`.
- Reels → `prompts/06-generate-reels-script.md`.
- Campaign → `prompts/07-generate-campaign.md`.
- Renderable → `prompts/13-generate-renderable-carousel.md` (chained after content).

Save raw output to `outputs/[client-slug]/.../raw/`.

---

## Stage 3 — Internal QA

In a *separate* chat / context:

```
Run prompts/08-critique-output.md on the output below. Be adversarial.
[paste]
```

Save the critique to `outputs/[client-slug]/.../qa/critique-iteration-1.json`.

If `Rewrite required`:
- Apply highest-leverage fix via `prompts/09-improve-output.md`.
- Re-run critique.
- Loop max 2 times.

If `Ship-ready` or `Ship with flags`:
- Continue to client review.

---

## Stage 4 — Client review

Build the handoff per `prompts/11-create-client-delivery.md`:

```
Use prompts/11-create-client-delivery.md.
Client/brand:        [slug]
Deliverables:        [list]
Cycle context:       [month + theme]
Review SLA:          24-48h
Approval channel:    [shared doc / Notion / email]
Internal flags:      [list]
```

Send via the agreed channel. Set expectation for response.

Save to `outputs/[client-slug]/.../review/HANDOFF.md`.

---

## Stage 5 — Apply client feedback

Capture feedback in `outputs/[client-slug]/.../review/feedback.md` per `client-feedback-form.md`.

If feedback violates the Brand Pack or claims rules:
- Surface the conflict; don't silently comply.
- Propose alternatives (apply with flag / refresh Brand Pack / keep original).

Apply only the requested changes via `prompts/09-improve-output.md`. Don't rewrite uninvited sections.

---

## Stage 6 — Final QA

Quick re-pass on affected sections. Confirm Quality Checklist still passes.

---

## Stage 7 — Deliver

Save final to `outputs/[client-slug]/.../final/`.

For Mode 1 (manual):
- Send final assets (Markdown + image prompts + caption) to client.
- Client publishes.

For Mode 3 (renderable):
- Worker renders final PNGs.
- Asset URLs sent to client.

---

## Stage 8 — Capture proof

Within 72 hours of publishing:

- Update `brands/[client-slug]/proof-assets.md` if anything quotable surfaced.
- Update `brands/[client-slug]/examples.md` with the shipped artifact.
- Update `voice.md` with new vocabulary.
- Note in monthly report.

---

## Per-client storage layout

```
brands/[client-slug]/             ← Brand Pack (12 files + feedback log)
outputs/[client-slug]/
  YYYY-MM-DD-topic-slug/
    brief.md
    raw/
    qa/
    review/
      HANDOFF.md
      feedback.md
    final/
    assets/                       ← rendered PNGs / image generations
    notes.md                      ← internal-only notes
```

---

## Per-deliverable time

| Format | Internal time | Client review |
|---|---|---|
| Single carousel | 90-180 min | 24-48h |
| Reels script | 60-90 min | 24-48h |
| 7-day campaign | 4-6 hours | 48-72h |
| Monthly plan | 6-10 hours | 48-72h |

---

## Revision policy

Per `revision-rules.md`:

- 1 revision round included per deliverable.
- Additional rounds billed.
- Capture each round in `feedback.md`.

---

## Communication discipline

- Ack receipt of client feedback within working hours.
- Send the revised deliverable within 24-48h.
- If a revision uncovers a bigger issue, flag it explicitly rather than silently absorbing it.

---

## End-of-deliverable checklist

- [ ] Final asset(s) saved to `final/`.
- [ ] Internal notes captured.
- [ ] Brand Pack updated if proof was captured.
- [ ] Feedback logged.
- [ ] Next deliverable's brief queued.

---

## Multi-client capacity

For 5+ clients:

- One Claude Project per client (or one Project with strict per-chat brand context).
- Standardize the deliverable folder structure (above).
- Reviewer rotation across team to avoid self-approval bias.
- Consider Mode 2 for production-line parts (carousel-machine, quality-gate).
