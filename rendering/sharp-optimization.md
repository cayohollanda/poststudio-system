# Sharp Optimization

> Post-render PNG/JPG optimization. Reduce file size without visible loss; ensure consistent output.

`sharp` (Node, libvips-based) is the recommended optimizer. Equivalents: `imagemin`, `oxipng`, `mozjpeg`.

---

## Pipeline

```javascript
import sharp from 'sharp';

async function optimizePng(buffer) {
  return sharp(buffer)
    .png({
      compressionLevel: 9,         // 0-9; 9 is max compression
      palette: false,              // keep full color
      adaptiveFiltering: true,     // better compression on photographic images
      effort: 7,                   // 1-10; higher = slower & smaller
    })
    .toBuffer();
}

async function optimizeJpg(buffer) {
  return sharp(buffer)
    .jpeg({
      quality: 90,                 // 80-95 is typical for hero PNGs as JPG
      progressive: true,
      mozjpeg: true,               // use mozjpeg encoder if available
      chromaSubsampling: '4:4:4',  // preserve color fidelity
    })
    .toBuffer();
}
```

---

## Quality settings per quality bias

| Quality bias | PNG settings | JPG settings | Typical size (1080×1350) |
|---|---|---|---|
| `draft` | `compressionLevel: 6, effort: 4` | `quality: 80, progressive: true` | 200-350 KB |
| `standard` | `compressionLevel: 9, effort: 7` | `quality: 90, progressive: true, mozjpeg: true` | 300-500 KB |
| `high` | `compressionLevel: 9, effort: 10` | `quality: 95, progressive: true, mozjpeg: true, chromaSubsampling: '4:4:4'` | 500-800 KB |

Quality bias comes from the request's `options.quality`.

---

## Format choice

Default: PNG.

Switch to JPG when:
- The carousel uses photographic backgrounds (e.g. Cinematic Dark with hero photo).
- File size matters (e.g. paid social where load time affects performance).
- The brand permits JPG (some brand guidelines prefer PNG for sharper text).

Never switch to JPG if:
- Headlines are on flat backgrounds (compression artifacts visible).
- The Brand Pack `visual-style.md` mandates PNG.

---

## Color handling

- Preserve sRGB color profile.
- Don't convert to CMYK.
- Don't strip ICC profiles unless the worker has explicit policy.

---

## Sharpening

For text-heavy slides (Editorial Minimal, Cinematic Dark with bottom-third headlines):

```javascript
sharp(buffer)
  .sharpen({ sigma: 0.5, m1: 0.5, m2: 0.5 })  // subtle sharpening for screen
  .png(...)
```

Sharpen *after* resize, not before. For full-resolution output (no resize), sharpening is optional.

---

## Resize (if needed)

If the worker generates at 2x and downsamples to 1x:

```javascript
sharp(buffer)
  .resize(1080, 1350, { fit: 'fill', kernel: 'lanczos3' })
  .png(...)
```

`lanczos3` produces sharper text than `nearest` or `cubic`.

For typical 1x rendering, skip resize entirely.

---

## Output validation

After optimization:

- Confirm file size below platform limit (LinkedIn carousel ~5MB per image; Instagram ~30MB).
- Confirm dimensions match canvas.
- Confirm format matches request.
- Compute SHA-256 checksum (for audit / dedupe).

```javascript
const checksum = createHash('sha256').update(optimizedBuffer).digest('hex');
```

---

## Common pitfalls

| Pitfall | Fix |
|---|---|
| Text looks blurry after optimize | Reduce sharpening / disable; verify dimensions |
| File size still huge | Reduce `effort`, switch to JPG, or resize |
| Color shift visible | Check ICC profile preservation |
| PNG has alpha channel where it shouldn't | Use `.flatten({ background: '#0E0F12' })` to remove alpha |

---

## Performance

`sharp` typical perf on a single 1080×1350 PNG:

- `effort: 4` → ~80ms.
- `effort: 7` → ~150ms.
- `effort: 10` → ~400ms.

Tune `effort` per quality bias. Don't over-spend on draft renders.

---

## Concurrency

`sharp` releases the V8 thread during native work; you can run many in parallel. Limit to CPU cores to avoid contention.

```javascript
const optimized = await Promise.all(
  rendered.map(buf => optimizePng(buf))
);
```
