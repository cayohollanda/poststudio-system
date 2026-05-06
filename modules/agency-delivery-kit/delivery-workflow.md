# Delivery Workflow

> The end-to-end production process. Per deliverable.

---

## Stages

```
Brief → Generate → Internal QA → Client review → Apply feedback → Final QA → Deliver
```

---

## Stage 1 — Brief

- Reference the monthly content plan; pick the next deliverable.
- Confirm any inputs needed are in place (`brands/[slug]/proof-assets.md`, etc.).
- Run any clarifying questions internally first; only escalate to the client if structurally needed.

## Stage 2 — Generate

- Run the appropriate prompt:
  - Carousel → `prompts/04-generate-carousel.md`
  - Reels → `prompts/06-generate-reels-script.md`
  - Campaign → `prompts/07-generate-campaign.md`
  - Renderable → `prompts/13-generate-renderable-carousel.md`
- Save raw output under `outputs/[client-slug]/[YYYY-MM-DD]-[topic-slug]/`.

## Stage 3 — Internal QA

- Run `prompts/08-critique-output.md` in a separate context (not the same as generation).
- Run `prompts/09-improve-output.md` if Quality Gate flags critical fails.
- Loop max 2 iterations.
- For renderable output: run renderability checks before any render attempt.

## Stage 4 — Client review

- Send via the agreed channel (shared doc, Notion, email).
- Use `handoff-template.md`.
- Set the SLA expectation: typical 24-48h; faster only if pre-arranged.

## Stage 5 — Apply feedback

- Capture client feedback in `client-feedback-form.md`.
- Apply only the requested changes; don't rewrite uninvited sections.
- If feedback violates the Brand Pack or claims rules, surface the conflict — don't silently comply.
- Loop max 1 revision round per `revision-rules.md`.

## Stage 6 — Final QA

- Quick re-pass of the affected sections.
- Confirm the Quality Checklist still passes after revision.

## Stage 7 — Deliver

- Send the final asset (Markdown / JSON / rendered PNGs).
- Save to `outputs/[client-slug]/[YYYY-MM-DD]-[topic-slug]/final/`.
- Update `brands/[client-slug]/examples.md` with the shipped artifact (after publish).

---

## Time budget per deliverable

| Deliverable | Internal time |
|---|---|
| Single carousel | 90-180 min |
| Single reels script | 60-90 min |
| 7-day campaign | 4-6 hours |
| 30-day plan | 6-10 hours |
| Brand pack refresh | 2-4 hours |

These are *internal* times. Add client review time on top.

---

## Where things live

```
outputs/
  [client-slug]/
    YYYY-MM-DD-topic-slug/
      raw/                  # first generation
      qa/                   # critique + improve outputs
      review/               # client-review version
      final/                # final delivered version
      assets/               # rendered PNGs / image generations
      notes.md              # decisions, gotchas, hand-off notes
```

---

## Handover discipline

When passing a deliverable between team members:

- Use `handoff-template.md`.
- Include the brief, the generation, the QA pass, the client feedback (if any).
- Don't assume the next person remembers context from a Slack thread.
