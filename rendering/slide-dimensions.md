# Slide Dimensions

> Canvas sizes, safe margins, platform variants.

---

## Default canvas

`1080 × 1350` pixels (4:5 aspect ratio).

Configurable via the request's `canvas` field. Worker honors the request; ignores the renderable's internal dimensions if they differ (sanity rule — request wins).

---

## Per-platform dimensions

| Platform | Dimensions | Aspect | Notes |
|---|---|---|---|
| LinkedIn (carousel doc) | 1080 × 1350 | 4:5 | Document-uploaded carousel |
| LinkedIn (image carousel) | 1200 × 1500 | 4:5 | Higher resolution if upgrading |
| Instagram | 1080 × 1350 | 4:5 | Sweet spot for feed |
| Instagram square | 1080 × 1080 | 1:1 | When 4:5 is forbidden by brand |
| TikTok carousel | 1080 × 1440 | 3:4 | Slightly taller than 4:5 |
| X (carousel) | 1600 × 900 | 16:9 | Wider; lower text density |
| X (single image) | 1080 × 1080 | 1:1 | Default |
| Threads | 1080 × 1350 | 4:5 | Mirror Instagram |
| Pinterest | 1000 × 1500 | 2:3 | Vertical; rare in carousels |

---

## Safe margins

Default: 48 pixels on all sides.

The headline + brand mark + slide number live within the safe area. Platform UI overlays (mute icons, slide indicators) typically stay outside, but margins protect against edge cases.

```
┌──────────────────────────────────────┐
│  ┌────────────────────────────────┐  │
│  │                                │  │
│  │       active content area      │  │
│  │       (canvas - 96px)          │  │
│  │                                │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
   48px safe margin on each side
```

Per-brand override allowed (set in `visual-style.md`). Don't go below 32px.

---

## Headline placement zones

Within the safe area:

| Zone | Coordinates (1080×1350 canvas, 48px margin) |
|---|---|
| Bottom-third | y: 900-1302, x: 48-1032 |
| Top-third | y: 48-450, x: 48-1032 |
| Left-third | y: 48-1302, x: 48-360 |
| Right-third | y: 48-1302, x: 720-1032 |
| Center | y: 480-870, x: 48-1032 |

The Brand Pack's `visual-style.md` defines the default zone per brand.

---

## Brand mark and slide number positions

| Element | Default position | Coordinates |
|---|---|---|
| Brand mark | bottom-right small | x: 1032 - mark_width, y: 1302 - mark_height (anchored bottom-right) |
| Slide number | top-right | x: 1032, y: 62 (anchored top-right, 14px from edge) |

---

## Aspect-ratio adaptation

When a request specifies a non-standard aspect ratio, the worker:

1. Adjusts the canvas dimensions.
2. Re-validates that headline + safe margins fit.
3. Re-positions the brand mark / slide number proportionally.

If the renderable's `payload.html` hardcodes 1080×1350 dimensions but the request asks for 1080×1080:

- Worker can re-render at 1080×1080 with the same body.
- Layout may break (fixed 1350px height in CSS).
- Solution: Claude should generate body with `width: <canvas.width>px; height: <canvas.height>px;` derived from the request's canvas, not hardcoded.

---

## Multi-canvas jobs

Some jobs request the same carousel rendered at multiple aspect ratios:

```json
{
  "render_targets": [
    { "platform": "linkedin", "canvas": { "width": 1080, "height": 1350 } },
    { "platform": "instagram_square", "canvas": { "width": 1080, "height": 1080 } }
  ]
}
```

The worker:

1. Renders the renderable carousel once per target.
2. Each render is a separate render job under the hood (or a multi-target single job, depending on architecture).
3. Returns asset URLs grouped per target.

---

## Resolution

Default render resolution: 1× (canvas pixels = output pixels).

For high-DPI hero slides:

- Render at 2× internally.
- Downsample to 1× via `sharp` with `lanczos3`.
- Trade compute for sharper text on Retina displays.

Not necessary for standard production. Reserve for hero / launch slides.

---

## Validation

Worker validates after render:

- Output dimensions match canvas.
- File size within platform limits.
- No transparent pixels at edges (unless intended).
- No unexpected aspect ratio drift.

If any fails → repair or fail loud.
