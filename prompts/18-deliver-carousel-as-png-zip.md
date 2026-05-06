# Prompt — 18 Deliver Carousel as PNG Zip (Mode 1, Claude Project)

> When the user is in a Claude Project (Mode 1) and asks for the carousel "as images," "as PNG," "as a zip," or "send the images here," **do not return an interactive Artifact**. Use code execution to render real PNG files and attach a downloadable `.zip`.
>
> This prompt only applies to **Mode 1** (Claude.ai Project / chat). For Mode 2/3, use `prompts/13-generate-renderable-carousel.md` + `prompts/17-export-render-job.md` and let the rendering worker produce the PNGs.

---

## When to use

The user has already received (or is about to receive) a carousel via `prompts/04-generate-carousel.md`, and now says one of:

- "create the images and send here"
- "crie as imagens e envie aqui"
- "render this as PNG"
- "give me the carousel as a .zip"
- "export the slides"
- "I want the actual images"

Then this prompt is the right path.

---

## Why a separate prompt

In a previous shipped session (referenced in changelog), Claude received this exact request and responded by building an **interactive React/SVG Artifact** — beautiful, but **not** what was asked for. The user wanted **downloadable PNG files**. They couldn't post an Artifact to LinkedIn.

This prompt prevents that failure mode.

---

## Hard rules (non-negotiable)

1. **Do not return an Artifact.** No `<artifact>`, no React component, no interactive HTML preview. The deliverable is a `.zip` file.
2. **Use the analysis / code-execution tool** (Python sandbox) to render and package.
3. **Never redraw brand marks/logos via SVG primitives.** Always read the actual asset file from `brands/[slug]/{mark,favicon,lockup,svg}/` (uploaded as Project Knowledge) and embed it as base64 in the SVG/HTML, OR composite it via PIL.
4. **Render at the brand's canvas dimensions** (default 1080×1350) at full fidelity.
5. **Use the brand's actual fonts** (Manrope for `weaura-ai`). Download from Google Fonts inside the sandbox if needed; embed via `@font-face` data URI.
6. **Apply all brand-style rules** from `brands/[slug]/visual-style.md`: palette (exact hex), typography, accent rule (one accent per slide), logo placement, margins.
7. **Output a `.zip` file** with naming: `[brand-slug]-[topic-slug]-[YYYY-MM-DD].zip` containing `slide-01.png` through `slide-NN.png`.
8. **Attach the zip as a downloadable file in the response.** Don't paste the file path; attach it.
9. **After attaching, echo a 5-line summary**: brand · topic · slide count · canvas · output filename.
10. **Never invent assets that don't exist.** If a brand asset path doesn't resolve, mark `[ASSET_NOT_FOUND]` in the response and use a flat brand-color rectangle as a placeholder. Surface to the user.

---

## Process (sandbox-friendly path)

The Claude.ai sandbox supports Python with `Pillow`, `cairosvg` (often), `requests`, `zipfile`. It typically does **not** have Playwright or a headless browser. So the canonical path is:

### Path A — SVG → PNG via cairosvg (preferred)

1. **Read each brand asset file** Claude needs:
   - `brands/[slug]/svg/mark-primary.svg` (or `mark-dark.svg` per the slide's background)
   - `brands/[slug]/svg/lockup-*.svg` (for slides that show the wordmark + mark)
   - Any image asset referenced by `assets_required[]`.
   Encode as base64 strings inside the Python sandbox.
2. **Build one `<svg>` document per slide** at exactly canvas dimensions (e.g. 1080×1350):
   - Background: solid brand color (e.g. `#0B0B0A` or `#FAF8F3` per slide design).
   - Headline: `<text>` in Manrope (or brand headline face), manually wrapped via `<tspan>` per the brand's headline placement rule.
   - Support text: `<text>` in body face.
   - Brand mark: `<image href="data:image/svg+xml;base64,...">` — the **real** asset, not a redraw.
   - Accent: applied to ONE element per slide (single word in headline, the mark dot, or one detail).
3. **Convert each SVG → PNG** via `cairosvg.svg2png(bytestring=svg_bytes, output_width=1080, output_height=1350)`.
4. **Optimize** via Pillow (`Image.open` → `.save(optimize=True)`).
5. **Package**: write all PNGs to a temp dir, zip via `zipfile.ZipFile(...)`, save to a stable output path.
6. **Attach** the zip as a file in the response.

### Path B — Direct Pillow composition (fallback if cairosvg missing)

If `cairosvg` isn't available in the sandbox:

1. For each slide, create a blank `Image.new("RGB", (1080, 1350), brand_bg_hex)`.
2. Use `ImageDraw.text()` with the brand font (download Manrope `.ttf` files from Google Fonts to sandbox, load via `ImageFont.truetype`).
3. Composite the brand mark via `Image.open(mark_png_path)` resized + `.paste()` with the alpha channel preserved.
4. Manually wrap headline text using `textwrap.wrap()` calibrated to the canvas width minus margins.
5. Save each slide as PNG, package as zip, attach.

Path B is lower fidelity for complex layouts but always works.

---

## Asset access in Claude.ai Project

When the repo (including `brands/[slug]/`) is uploaded as Project Knowledge, the binary assets (`.svg`, `.png`) are accessible via:

- The Files API inside the sandbox — `/mnt/user-data/...` or similar.
- Or directly read by Claude and passed as bytes/strings into the sandbox via the conversation.

If the binary asset isn't accessible (some Project Knowledge configurations only expose text), Claude **must**:

1. Tell the user: "I can see the brand pack but the binary assets aren't readable in this session. Please upload `mark-primary.svg` and `lockup-on-dark.svg` directly into this chat as files."
2. Wait for the upload.
3. Then proceed.

**Do not redraw the mark from scratch as a fallback.** That's what produced the bug.

---

## Per-slide builder function (reference)

```python
import cairosvg, base64, zipfile, io
from pathlib import Path

def build_slide_svg(slide, brand, mark_b64):
    # slide = {slide_number, role, headline, support_text, background, accent_word}
    bg = "#0B0B0A" if slide["background"] == "dark" else "#FAF8F3"
    ink = "#F2EFE8" if slide["background"] == "dark" else "#0B0B0A"
    accent = brand["accent"]  # e.g. "#CCFF00"

    # Manually wrap headline at brand's max chars per line
    lines = wrap_headline(slide["headline"], max_chars=24)
    tspans = "\n".join(
        f'<tspan x="48" dy="{0 if i==0 else 90}">{escape(line)}</tspan>'
        for i, line in enumerate(lines)
    )

    return f'''<svg xmlns="http://www.w3.org/2000/svg" width="1080" height="1350" viewBox="0 0 1080 1350">
  <rect width="1080" height="1350" fill="{bg}"/>

  <!-- BRAND MARK — embedded base64, NOT redrawn -->
  <image x="48" y="48" width="56" height="56"
         xlink:href="data:image/svg+xml;base64,{mark_b64}"/>

  <!-- HEADLINE -->
  <text x="48" y="900"
        font-family="Manrope, sans-serif"
        font-weight="600"
        font-size="76"
        fill="{ink}"
        letter-spacing="-1">
    {tspans}
  </text>

  <!-- SUPPORT -->
  <text x="48" y="1180"
        font-family="Manrope, sans-serif"
        font-weight="400"
        font-size="28"
        fill="{ink}"
        opacity="0.75">{escape(slide["support_text"])}</text>

  <!-- SLIDE NUMBER -->
  <text x="1032" y="80"
        font-family="ui-monospace, monospace"
        font-size="20"
        fill="{ink}"
        opacity="0.5"
        text-anchor="end">{slide["slide_number"]:02} / {slide["total"]:02}</text>
</svg>'''

def render_zip(slides, brand_slug, topic_slug, mark_path):
    mark_b64 = base64.b64encode(Path(mark_path).read_bytes()).decode()
    zip_path = f"/tmp/{brand_slug}-{topic_slug}.zip"
    with zipfile.ZipFile(zip_path, "w") as z:
        for slide in slides:
            svg = build_slide_svg(slide, brand={"accent": "#CCFF00"}, mark_b64=mark_b64)
            png = cairosvg.svg2png(bytestring=svg.encode(), output_width=1080, output_height=1350)
            z.writestr(f"slide-{slide['slide_number']:02}.png", png)
    return zip_path
```

This is a starting skeleton. Adapt to the brand's specific layout per `visual-style.md`.

---

## Layout rules to honor (from the brand pack)

For `weaura-ai` specifically:

- Canvas: 1080×1350.
- Backgrounds alternate per the carousel plan (DARK `#0B0B0A` or LIGHT `#FAF8F3`).
- Headline: Manrope SemiBold (600), 60-80px, sentence case, letter-spacing tight.
- Support: Manrope Regular (400), 22-28px, neutral gray.
- Accent VOLT `#CCFF00`: one element per slide max (one word in headline, the mark's central dot, or one graphic detail).
- Brand mark: `mark-primary` on dark bg, `mark-dark` on light bg. Always embedded from `brands/weaura-ai/svg/`.
- Lockup on slide 1 + final CTA slide; mark-only on intermediate slides if used.
- Margins: 48px on all sides.

Refer to `brands/weaura-ai/visual-style.md` and the asset pack at `brands/weaura-ai/svg/` for the canonical values.

---

## Output template

After running the sandbox code:

```
✅ Carousel rendered as PNGs.

- Brand: weaura-ai
- Topic: <one-line topic>
- Slide count: 8
- Canvas: 1080×1350
- File: weaura-ai-<topic-slug>-2026-05-06.zip

[attached: weaura-ai-<topic-slug>-2026-05-06.zip]

If anything in the rendering looks off (font fallback, asset not found, overflow),
tell me which slide and I'll re-render that one.
```

If asset access failed:

```
⚠️ I couldn't access the binary brand assets in this session.

Please drop these files directly into the chat and I'll re-render:
- brands/weaura-ai/svg/mark-primary.svg
- brands/weaura-ai/svg/mark-dark.svg
- brands/weaura-ai/svg/lockup-transparent-cream.svg

(I refuse to redraw the aperture mark from primitives — it would be wrong.)
```

---

## What you must NEVER do in this prompt

- ❌ Return an interactive Artifact (React/SVG/HTML preview).
- ❌ Redraw the brand mark / logo / aperture via SVG primitives.
- ❌ Invent the brand's assets if they aren't accessible — ask the user to upload them.
- ❌ Use generic fonts when the Brand Pack specifies a real one — download or attach Manrope.
- ❌ Use multiple accent colors per slide.
- ❌ Skip the zip step — even one PNG should ship as `slide-01.png` inside a zip for consistency.
- ❌ Output the file path as text — actually attach the file.

---

## Pairing with the carousel prompt

The natural sequence in Mode 1:

1. **`prompts/04-generate-carousel.md`** → carousel content (Markdown).
2. *User: "create the images and send here"* → **this prompt (`18-deliver-carousel-as-png-zip.md`)**.
3. *User: "this looks great, run quality gate"* → `prompts/08-critique-output.md`.

Or for the renderable-first path:

1. **`prompts/04-generate-carousel.md`** → carousel content.
2. **`prompts/13-generate-renderable-carousel.md`** → renderable HTML/CSS/SVG.
3. **This prompt** to convert the renderable into a downloadable zip.

If the user is on Mode 2/3 with a real worker, skip this prompt entirely — the worker handles rendering.
