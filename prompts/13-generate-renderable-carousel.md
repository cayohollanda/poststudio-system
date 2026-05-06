# Prompt — 13 Generate Renderable Carousel

> Produces a renderable carousel (HTML/CSS, SVG, or template-driven JSON) ready for a worker to render into PNG/JPG slides.

---

## Prompt

```
You are operating PostStudio System.

Generate a renderable carousel using:
- modules/renderable-creative-system/instructions.md
- system/renderable-creative-framework.md
- modules/renderable-creative-system/<sub-mode>-rules.md (html-css | svg | template)
- system/api-output-rules.md
- system/safety-and-claims-rules.md
- The active Brand Pack for this session, especially visual-style.md (supplied via prompts/19-ephemeral-brand-intake.md OR via a brands/[BRAND_SLUG]/ folder the user uploaded as Project Knowledge — never loaded from the framework repo)

# Inputs

- Brand: [BRAND_SLUG] — session-supplied brand pack (12 sections in-memory) or user-uploaded brands/[BRAND_SLUG]/ folder
- Topic: [one-sentence topic]
- Audience: [role + situation]
- Platform: [linkedin / instagram / tiktok / x]
- Language: [en / pt / es / ...]
- Goal: [save / comment / share / click / dm / follow / try]
- Render mode: [html-css | svg | template]
- Canvas: [{ width: 1080, height: 1350, format: "png" }]
- Available fonts: [{ family, fallback_chain }, ...]
- Available assets: [{ asset_id, purpose, required }, ...]
- Available templates (template mode only): [{ template_id, role, slots: { ... } }, ...]
- Existing carousel content (optional): [paste a content carousel JSON to convert]

# Process

1. If existing content is provided: skip content generation; convert directly.
   If not: generate content first per modules/carousel-machine/instructions.md, then convert.
2. For each slide:
   - Map content to the chosen sub-mode's payload shape.
   - HTML/CSS: write a self-contained document per modules/renderable-creative-system/html-css-rules.md.
   - SVG: write a valid <svg> per modules/renderable-creative-system/svg-rules.md.
   - Template: pick template_id from available_templates; fill slots per modules/renderable-creative-system/template-json-rules.md.
3. Add slide metadata (role, headline, support_text, accent_color, brand_slug).
4. Aggregate fonts_required and assets_required at the carousel level.
5. Carry through caption, CTA, first comment, alt hooks (these are not renderable; separate JSON fields).
6. Run renderability self-checks per modules/renderable-creative-system/render-quality-checklist.md.
7. Set renderability_validated: true only if all critical checks pass.

# Output

Return ONLY valid JSON conforming to schemas/renderable-carousel.schema.json. No surrounding commentary, no Markdown fences.

If you cannot produce safe renderable output, return:

{
  "version": "1.0",
  "prompt_version": "v2.0.0",
  "request_id": "...",
  "errors": [{ "code": "UNSAFE_RENDERABLE", "message": "...", "remediation": "..." }]
}

# Hard rules

- No JavaScript anywhere.
- No remote URLs (only data: URIs and {{asset:<id>}} placeholders).
- No invented logos or fake brand assets.
- Canvas matches request.
- Safe margins (default 48px).
- Fallback fonts in every text element.
- One accent color per slide, applied selectively.
- Deterministic layout.
- Editable text (no rasterized text in <img>).
```

---

## Example invocation

```
Brand: brands/lumen/
Topic: most AI tools fail at deep work
Audience: senior PMs and engineering leads at 50-500 person SaaS
Platform: linkedin
Language: en
Goal: save
Render mode: html-css
Canvas: { width: 1080, height: 1350, format: "png" }
Available fonts: [
  { family: "GT Sectra Display", fallback_chain: ["Tiempos Headline", "Georgia", "serif"] },
  { family: "Söhne", fallback_chain: ["Inter", "system-ui", "sans-serif"] },
  { family: "IBM Plex Mono", fallback_chain: ["Menlo", "monospace"] }
]
Available assets: [
  { asset_id: "fractured-glass-dome-01", purpose: "slide-1 background", required: true }
]
Existing content: [paste prior carousel-machine output]
```

Claude returns the renderable carousel JSON.

---

## When to use

- Mode 3 production pipeline.
- After a content carousel has been approved by Quality Gate.
- For one-off renderable explorations.

---

## When NOT to use

- Mode 1 (manual layout in Figma) — skip; use the standard carousel output.
- Content hasn't been approved yet — run `prompts/04-generate-carousel.md` first.

---

## Iterating

For repair, use [`prompts/16-repair-renderable-output.md`](16-repair-renderable-output.md).

For a fresh attempt with different sub-mode:

```
Re-run with render_mode: "template" using the available_templates above. Map slide 1 to lumen.cinematic-dark.hook.v1 and slide 8 to lumen.cinematic-dark.cta.v1.
```
