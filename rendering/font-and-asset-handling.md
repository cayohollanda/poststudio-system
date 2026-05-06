# Font and Asset Handling

> The worker's font registry and asset registry. How they're populated, accessed, and resolved at render time.

---

## Font registry

The worker maintains a registry of available fonts:

```
fonts/
  GT Sectra Display/
    GT-Sectra-Display-Medium.woff2
    metadata.json   # family, weight, style, license
  Söhne/
    Soehne-Regular.woff2
    Soehne-Medium.woff2
    metadata.json
  IBM Plex Mono/
    IBMPlexMono-Regular.woff2
    metadata.json
  ... system fallbacks always available
```

`metadata.json`:

```json
{
  "family": "GT Sectra Display",
  "files": [
    { "weight": 500, "style": "normal", "path": "GT-Sectra-Display-Medium.woff2" }
  ],
  "license": "commercial",
  "tenants_allowed": ["tenant_lumen"]
}
```

---

## Per-tenant font registration

Some fonts are licensed per-tenant. The registry checks `tenants_allowed` before injecting:

```javascript
function fontsForTenant(tenantId, requestedFonts) {
  return requestedFonts
    .filter(f => isAllowedForTenant(f.family, tenantId))
    .map(f => loadFontFile(f.family));
}
```

If a font isn't licensed for the tenant → fall back to the chain; emit warning.

---

## System fallbacks

Always available (not licensed; widely shipped):

- `Georgia, serif`
- `Times New Roman, serif`
- `Helvetica, Arial, sans-serif`
- `Courier New, monospace`
- `system-ui` (sans-serif default)
- Generic families: `serif`, `sans-serif`, `monospace`

The fallback chain in every renderable text element ends in one of these.

---

## Font injection at render time

The worker reads `fonts_required[]` from the renderable carousel:

```json
"fonts_required": [
  { "family": "GT Sectra Display", "fallback_chain": ["Tiempos Headline", "Georgia", "serif"] }
]
```

For each requested font:

1. Check tenant allowance.
2. Load the font file from the registry.
3. Inject `@font-face` rule into the rendered HTML / SVG wrapper.

```css
@font-face {
  font-family: "GT Sectra Display";
  src: url("data:font/woff2;base64,<base64>") format("woff2");
  font-display: block;
  font-weight: 500;
  font-style: normal;
}
```

`font-display: block` ensures the renderer waits for the font before rendering text.

---

## Asset registry

The worker maintains a registry of approved brand assets:

```
assets/
  lumen/
    fractured-glass-dome-01.png
    calendar-pages-fragmenting-02.png
    metadata.json
```

`metadata.json`:

```json
{
  "asset_id": "fractured-glass-dome-01",
  "brand_slug": "lumen",
  "url": "https://cdn.crud.example/lumen/fractured-glass-dome-01.png",
  "license": "owned",
  "checksum": "sha256:7f3c2a1d...",
  "uploaded_at": "2026-04-15T...",
  "tags": ["slide-1-bg", "cinematic-dark", "hero"]
}
```

---

## Asset resolution

When the worker encounters `{{asset:<id>}}`:

1. Look up `<id>` in the brand's asset folder.
2. If found:
   - Strategy A: replace with the resolved URL (allowed-domain check).
   - Strategy B: pre-fetch and inline as `data:` URI (size-capped).
3. If not found: strip and fall back to flat color; emit `ASSET_NOT_FOUND` warning.

Cross-brand asset access is forbidden. Tenant `lumen` cannot reference `lumen-competitor`'s assets.

---

## Asset registration (admin path)

When a brand uploads a new asset:

1. Validate file format (PNG, JPG, WebP).
2. Scan for malware / unexpected content.
3. Compute checksum.
4. Strip metadata (EXIF, etc.) for privacy.
5. Optimize (sharp).
6. Upload to CDN.
7. Register in `assets/<brand-slug>/`.
8. Emit asset registry update event.

---

## Asset URL signing

For private brand assets:

- Asset URLs are signed with short-lived tokens (e.g. AWS S3 presigned URL with 5min TTL).
- The worker re-signs at render time.
- Signed URLs not stored in the renderable_carousel JSON (they expire); use `{{asset:<id>}}` placeholder.

---

## Naming conventions

Asset IDs:

```
<brand-slug>.<purpose>.<variant>.<sequence>
```

Examples:

- `lumen.slide-1-bg.fractured-dome.01`
- `lumen.proof-slide.case-study-vp-eng.01`
- `lumen.cta-slide.brass-key.01`

Predictable IDs make Claude's asset selection less ambiguous.

---

## Failure modes

| Failure | Handling |
|---|---|
| Font not in registry | Substitute via chain; emit warning |
| Font not licensed for tenant | Substitute via chain; emit warning |
| Asset not in registry | Strip; flat color fallback; emit warning |
| Asset 404 (registry says it's there but CDN miss) | Retry once; then strip; emit error |
| Asset wrong dimensions for slide | Render with `object-fit: cover` (CSS) or `preserveAspectRatio` (SVG); emit warning if cropping is heavy |

---

## Audit

Every render logs:

- Fonts injected (family, version).
- Assets resolved (id, checksum, source).
- Substitutions / fallbacks emitted.

Audit trail enables reproducibility. If the same job is re-run later, identical inputs (including registry state) produce identical outputs.
