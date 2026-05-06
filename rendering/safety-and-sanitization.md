# Safety and Sanitization

> The worker's defense layer. Sanitizes all incoming HTML/CSS/SVG before rendering.

The renderable contract limits what Claude *should* produce. Sanitization enforces what the worker *will* execute.

---

## Threat model

The worker must defend against:

1. **Prompt-injected content** (Claude was tricked into producing malicious markup).
2. **Brand Pack tampering** (a tenant pushes harmful content via Brand Pack).
3. **Asset registry poisoning** (a malicious asset URL).
4. **Renderer escape** (an SSRF or RCE in the headless browser).
5. **Resource exhaustion** (rendering a payload designed to OOM the worker).

---

## HTML/CSS sanitization

Use a hardened sanitizer (e.g. `DOMPurify`, `bleach`, `sanitize-html`) configured to:

### Allow
```
tags: html, head, body, style, meta (charset only),
      div, span, h1-h6, p, br, hr, a (with restricted href),
      img (with restricted src), strong, em, b, i, u,
      ul, ol, li, table (rare), tr, td, th
attrs: class, id, style, lang, dir, data-*, role, aria-*
       width, height (on img), alt (on img),
       href (a, restricted), src (img, restricted)
```

### Strip
```
tags: script, link, iframe, embed, object, form, input, button,
      audio, video, source, track, picture, source, base, meta (other than charset)
attrs: on*=, javascript:, eval, expression(),
       src/href starting with http://, https://,
       any attribute containing CSS expression()
```

### CSS sanitization
- Use a CSS sanitizer (e.g. CSS-tree based) to:
  - Strip `@import url(...)`.
  - Strip `url(http://...)` / `url(https://...)`.
  - Strip `expression()`.
  - Strip `javascript:` URIs.
  - Strip banned properties per [`html-css-generation-rules.md`](html-css-generation-rules.md).

---

## SVG sanitization

Use an SVG sanitizer (e.g. `DOMPurify` SVG profile, custom):

### Allow
```
elements: svg, defs, style, rect, circle, ellipse, line, path,
          polyline, polygon, text, tspan, g, image, use (local refs only),
          linearGradient, radialGradient, stop,
          filter, feGaussianBlur, feDropShadow, feColorMatrix,
          feComposite, feMerge, feMergeNode
attrs:    class, id, style, xmlns, viewBox, width, height,
          x, y, cx, cy, r, rx, ry, d, points, fill, stroke,
          stroke-width, opacity, transform, font-family,
          font-size, font-weight, text-anchor, dominant-baseline,
          dx, dy, href (image, restricted), xlink:href (image, restricted),
          preserveAspectRatio, gradientUnits, offset, stop-color, stop-opacity,
          filter, in, in2, result, stdDeviation, type, values, mode
```

### Strip
```
elements: script, foreignObject, animate*, set, audio, video,
          metadata (with embedded HTML), desc (with embedded HTML)
attrs:    on*, javascript:, http(s):// in href/xlink:href,
          any data-uri exceeding size cap
```

---

## Asset URL validation

When resolving `{{asset:<id>}}` to real URLs:

- The asset registry only stores URLs from approved domains (typically the brand's own CDN).
- Resolved URLs validated against the allowed-domain list before fetch.
- HTTP redirects: follow only one level, only within the allowed domain.

---

## Inline base64 size cap

Inline `data:` URIs can blow up the rendered DOM:

- Cap per URI: 500KB.
- Cap per slide: 2MB total inline data.
- Beyond cap → strip; emit warning; fall back to flat color.

---

## Rendering sandbox

The renderer process runs in a sandbox:

- Containerized (Docker / OCI).
- Network-isolated except for asset CDNs (or fully offline if assets are pre-fetched).
- File-system read-only except for the job's working directory.
- Memory limit (e.g. 1GB per render).
- CPU limit.
- Timeout per slide (e.g. 30s); kill on timeout.

For headless Chromium specifically:

- `--disable-javascript`.
- `--disable-extensions`.
- `--disable-plugins`.
- `--disable-gpu` (or enable selectively for performance).
- `--no-sandbox` only if the host provides equivalent isolation.

---

## Sanitization failure modes

| Failure | Action |
|---|---|
| Sanitizer strips an element | Render proceeds; `UNSAFE_HTML_FEATURE` warning emitted. |
| Sanitizer leaves the slide non-renderable (e.g. all `<img>` stripped) | Slide fails with `UNSAFE_HTML_FEATURE` error. |
| Asset URL not in allowed domains | Strip; fall back to flat color; warning. |
| Inline base64 exceeds size cap | Strip; fallback; warning. |
| SVG parser failure post-sanitization | Slide fails with `INVALID_SVG`. |

All sanitization actions are logged for audit.

---

## Test corpus

Maintain a corpus of malicious / pathological inputs to test the sanitizer:

- HTML with `<script>alert(1)</script>`.
- SVG with `<foreignObject><iframe>`.
- CSS with `expression(...)`.
- Inline 10MB base64 image.
- Recursive style imports.
- Comment-hidden `<script>` (e.g. `<!--<script>-->`).
- Encoded payloads (`&#x73;cript`).

Run on every release.

---

## Audit

Every render job logs:
- Sanitizer actions taken.
- Strip counts.
- Asset URL resolutions (with hash; not full URL for privacy).
- Sandbox memory / CPU usage.

Anomaly detection on these signals.
