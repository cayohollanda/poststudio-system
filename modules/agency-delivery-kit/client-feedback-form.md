# Client Feedback Form

> Structured capture for client review feedback. Used per deliverable. Logged to `brands/[client-slug]/feedback-log.md` for pattern analysis.

---

## Form

```markdown
# Feedback — [Brand] — [Deliverable] — [YYYY-MM-DD]

**Reviewer:** [client name + role]
**Deliverable:** [carousel / reel / campaign / plan]
**Reviewed at:** [URL or path]

## Decision

- [ ] Approved as-is — ship.
- [ ] Approved with the changes below.
- [ ] Needs revision (round 1 / round 2).
- [ ] Reject — start over (rare; document why).

## Changes requested

| # | Slide / Section | Change | Type |
|---|---|---|---|
| 1 | Slide 4 | Replace "destroy" with "interrupt" | tone |
| 2 | Caption | Add a sentence about [topic] | content |
| 3 | CTA | Change to "DM me '[KEYWORD]'" | strategy |

**Type vocabulary:** tone / content / strategy / fact / compliance / voice / visual / other

## Conflicts with Brand Pack

[Did any feedback contradict the Brand Pack? List + how it was handled.]

## Conflicts with claims rules

[Did any feedback request invented numbers / customers / awards? List + how it was handled.]

## Notes

[Any additional context the team should keep — e.g. "the founder is sensitive about word X", "we're building toward [topic] in next month's content"]

## Next step

- [ ] Apply changes and re-send for approval.
- [ ] Apply changes and ship directly.
- [ ] Schedule a call to align before changes.
- [ ] Escalate (Brand Pack refresh needed).
```

---

## Rules for capturing feedback

- **Capture verbatim where possible.** "Make it less corporate" is vague; capture the original phrasing.
- **Tag every change with a type.** Patterns reveal what to refresh in the Brand Pack.
- **Surface conflicts immediately.** Don't quietly resolve a Brand Pack contradiction.
- **Log the decision.** Approved / revised / rejected — explicit.

---

## When the client provides feedback in unstructured form (chat, voice memo)

Convert to this form yourself before applying. Don't apply changes ad-hoc.

If the client is averse to structured forms, capture in writing in your project doc and reference back.

---

## Feedback log per brand

```
brands/[client-slug]/feedback-log.md
```

Format:

```markdown
# Feedback log — [Brand]

## YYYY-MM-DD — [Deliverable]

[Compressed summary: 3 changes requested, 1 Brand Pack conflict resolved, 1 claim conflict held.]

[Patterns to watch:]
[...]
```

After 3-5 deliverables, the log shows pattern: "client always softens contrarian hooks" → either soften by default (Brand Pack update) or push back consistently.

---

## Feedback hygiene

- A client who can't articulate their feedback is the bottleneck. Help them with structure.
- A client who *only* gives positive feedback isn't reading carefully. Ask specific questions.
- A client who rewrites every sentence is doing your job. Push back kindly: "What's the underlying issue you'd like us to fix? We can address the structure rather than the words."
