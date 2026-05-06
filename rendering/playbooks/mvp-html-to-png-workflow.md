# Playbook — MVP HTML/CSS → PNG Workflow

> The minimum viable rendering pipeline. Stand this up first; iterate.

---

## Goal

Render a renderable carousel into PNG slides as fast as possible. Stateless, single-host, single-brand to start.

---

## Stack (minimum)

- Node.js worker process.
- Playwright (Chromium).
- `sharp` for optimization.
- S3 / R2 / GCS for storage.
- HTTP server (Express, Fastify, or framework of choice).

---

## Steps

### 1. Worker scaffold

```javascript
// src/worker.js
import express from 'express';
import { chromium } from 'playwright';
import sharp from 'sharp';
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';
import { sanitizeHtml } from './sanitize.js';

const browser = await chromium.launch({ headless: true, args: [...] });
const s3 = new S3Client({ region: 'us-east-1' });
const app = express();
app.use(express.json({ limit: '10mb' }));

app.post('/render-jobs', async (req, res) => {
  const job = req.body;
  // ... validate ...
  const result = await renderJob(job);
  res.status(202).json({ render_job_id: job.render_job_id, status: 'queued' });
  // POST to callback_url
  await postCallback(job.callback_url, result);
});

app.listen(8080);
```

### 2. Render loop

```javascript
async function renderJob(job) {
  const slides = job.renderable_carousel.slides;
  const canvas = job.renderable_carousel.carousel.canvas;
  const fonts = job.renderable_carousel.carousel.fonts_required;

  const results = await Promise.all(
    slides.map(slide => renderSlide({ slide, canvas, fonts }))
  );

  return buildRenderResult(job, results);
}

async function renderSlide({ slide, canvas, fonts }) {
  const { html } = slide.payload;
  const sanitized = sanitizeHtml(html);
  const resolved = resolveAssets(sanitized);

  const context = await browser.newContext({
    viewport: { width: canvas.width, height: canvas.height },
    javaScriptEnabled: false,
  });
  const page = await context.newPage();
  await page.route('**/*', (route) => {
    if (isAllowed(route.request().url())) return route.continue();
    return route.abort();
  });
  await page.addStyleTag({ content: buildFontFaceCss(fonts) });
  await page.setContent(resolved, { waitUntil: 'networkidle' });
  await page.evaluate(() => document.fonts.ready);
  const buf = await page.screenshot({ type: 'png' });
  await context.close();

  const optimized = await sharp(buf).png({ compressionLevel: 9 }).toBuffer();
  const url = await uploadToS3(optimized, job, slide.slide_number);

  return { slide_number: slide.slide_number, status: 'rendered', asset_url: url, ... };
}
```

### 3. Sanitizer

Use `DOMPurify` or `sanitize-html` configured per [`safety-and-sanitization.md`](../safety-and-sanitization.md).

### 4. Asset resolver

Replace `{{asset:<id>}}` with pre-fetched data URIs (simplest for MVP):

```javascript
function resolveAssets(html) {
  return html.replace(/\{\{asset:([^}]+)\}\}/g, (_, id) => {
    const asset = assetRegistry.get(id);
    if (!asset) return ''; // or fallback color
    return `data:${asset.mime};base64,${asset.base64}`;
  });
}
```

### 5. Upload

```javascript
async function uploadToS3(buffer, job, slideNumber) {
  const key = `tenants/${job.tenant_id}/brands/${job.brand_id}/jobs/${job.job_id}/assets/slide-${slideNumber}.png`;
  await s3.send(new PutObjectCommand({
    Bucket: 'crud-assets',
    Key: key,
    Body: buffer,
    ContentType: 'image/png',
  }));
  return `https://crud-assets.s3.amazonaws.com/${key}`;
}
```

### 6. Callback

```javascript
async function postCallback(url, result) {
  const body = JSON.stringify(result);
  const signature = hmacSha256(body, secret);
  await fetch(url, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-PostStudio-Signature': `sha256=${signature}`,
    },
    body,
  });
}
```

---

## Success criteria

The MVP works when:

- [ ] An 8-slide carousel renders in < 90 seconds.
- [ ] All slides are 1080×1350 PNG.
- [ ] Fonts render correctly (no fallback drift).
- [ ] Asset placeholders resolve.
- [ ] Output uploaded to S3.
- [ ] Callback fires.

---

## Next steps after MVP

- Add concurrency control (max N jobs in flight).
- Add asset registry caching.
- Add sanitization edge-case handling.
- Add Docker container.
- Add health endpoint.
- Add observability (per `observability-and-logging.md`).
- Add multi-target rendering.
- Layer in template-driven sub-mode.

---

## What you don't need at MVP

- Multi-tenant complexity (single tenant first).
- Template library (freeform first).
- SVG sub-mode (HTML/CSS first).
- Repair loops (manual debug first).
- Webhook signature verification (basic shared secret first).

Add these once the basic pipeline is humming.
