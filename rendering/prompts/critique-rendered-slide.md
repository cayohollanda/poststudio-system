# Prompt — Critique Rendered Slide (rendering reference)

> Specialized critique prompt for the *rendered* output (PNG/JPG), not the renderable JSON. Used in the post-render QA loop.

---

## When to use

- After a render completes, to inspect the actual output.
- For new brands' first 5 carousels (manual + automated review).
- When automated QA flags drift from the brand reference.
- For compliance-sensitive renders before publishing.

## Inputs

- The rendered slide (PNG/JPG).
- The intended renderable_carousel JSON (what we wanted).
- The Brand Pack.
- Optional: a reference image for the brand (for pHash drift detection).

## Output

A `quality-review` document scoped to the rendered output:

- Verdict: `Render Ship-ready` / `Render with flags` / `Re-render required` / `Re-generate from content`.
- Rendered-output checks (font fidelity, color fidelity, layout fidelity, text legibility, brand mark presence).
- Drift assessment vs. reference (if available).
- Recommended fix if any.

---

## Rendered-output checklist

- [ ] Output dimensions match canvas.
- [ ] No text overflow visible.
- [ ] Headline is readable at thumbnail size.
- [ ] Accent color appears as expected.
- [ ] Brand mark visible in expected position (if applicable).
- [ ] No garbage text from font fallback.
- [ ] Background image loaded (if expected).
- [ ] No transparent edges (unless intended).
- [ ] Slide reads as coherent at full and thumbnail size.

---

## When the rendered output fails QA

| Failure | Recommended fix |
|---|---|
| Visible text overflow | Re-render with shorter headline (`prompts/16-repair-renderable-output.md`) |
| Font fallback used (visible drift) | Register the font; re-render |
| Asset missing (flat color where image expected) | Surface; ask user to provide the asset |
| Color drift | Check sanitizer / optimizer settings; adjust |
| Brand mark missing | Re-render with mark placement instruction |

---

## Note: this is mostly a human-review prompt

Most of the rendered-output review is a human task today. This prompt exists for cases where the consumer wants Claude to produce a structured quality summary of an image — but Claude reading the rendered PNG isn't its strongest task.

For new brands, treat this as a guided review checklist for a human reviewer using Claude as a structured note-taker.

---

## See

- Quality assurance: [`rendering/quality-assurance.md`](../quality-assurance.md)
- Quality Gate (general): [`prompts/08-critique-output.md`](../../prompts/08-critique-output.md)
