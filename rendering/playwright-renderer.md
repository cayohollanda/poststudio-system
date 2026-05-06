# Playwright Renderer

> Reference implementation: HTML/CSS → PNG via headless Chromium / Playwright. Pseudocode and configuration.

This is the recommended renderer for Freeform HTML/CSS sub-mode. Other headless browsers (Puppeteer, Playwright with Firefox/WebKit) work the same way.

---

## Setup

```javascript
import { chromium } from 'playwright';

const browser = await chromium.launch({
  headless: true,
  args: [
    '--disable-javascript',
    '--disable-extensions',
    '--disable-plugins',
    '--disable-gpu',
    '--font-render-hinting=none',  // consistent text rendering
    '--hide-scrollbars',
    '--disable-features=AudioServiceOutOfProcess',
  ],
});
```

Launch once per worker process; reuse across renders. Restart browser every N renders to avoid memory creep.

---

## Per-slide render

```javascript
async function renderSlide({ html, canvas, fonts, sandboxOpts }) {
  const context = await browser.newContext({
    viewport: { width: canvas.width, height: canvas.height },
    deviceScaleFactor: 1,
    javaScriptEnabled: false,
  });
  const page = await context.newPage();

  // Block all network requests except approved hosts
  await page.route('**/*', (route) => {
    const url = route.request().url();
    if (isAllowedAssetUrl(url) || url.startsWith('data:')) {
      return route.continue();
    }
    return route.abort();
  });

  // Inject font @font-face rules into the page before navigation
  await page.addStyleTag({ content: buildFontFaceCss(fonts) });

  // Set the HTML content
  await page.setContent(html, {
    waitUntil: 'networkidle',
    timeout: sandboxOpts.timeoutMs,
  });

  // Wait for fonts
  await page.evaluate(() => document.fonts.ready);

  // Screenshot
  const buf = await page.screenshot({
    type: 'png',
    fullPage: false,
    omitBackground: false,
    clip: { x: 0, y: 0, width: canvas.width, height: canvas.height },
  });

  await context.close();
  return buf;
}
```

---

## Font injection

The worker maintains a font registry. Build `@font-face` CSS at injection time:

```javascript
function buildFontFaceCss(fonts) {
  return fonts
    .map(f => `
      @font-face {
        font-family: "${f.family}";
        src: url("data:font/woff2;base64,${f.base64}") format("woff2");
        font-display: block;
      }
    `)
    .join('\n');
}
```

This guarantees the font is available without network access.

---

## Asset injection

Two strategies:

### A) Pre-resolved URLs

The worker resolves `{{asset:<id>}}` to a real URL before navigation. Allowed-domain check + network rule allows only those URLs.

### B) Pre-fetched data URIs

The worker fetches each asset, converts to base64, and replaces `{{asset:<id>}}` with `data:image/png;base64,...`. Eliminates network during render. Slower setup; safer.

For air-gapped renders, use B. For typical setups, A.

---

## Concurrency

Each `chromium.newContext()` is cheap; each `chromium.launch()` is expensive. Pattern:

- One `chromium` per worker process.
- One `context` per render job.
- One `page` per slide within the job.

```javascript
async function renderJob(job) {
  const slides = await Promise.all(
    job.slides.map(slide =>
      pool.run(() => renderSlide({
        html: slide.payload.html,
        canvas: job.carousel.canvas,
        fonts: job.fonts_required,
        sandboxOpts: { timeoutMs: 30000 },
      }))
    )
  );
  return slides;
}
```

`pool` limits concurrent slides per job (default 4).

---

## Determinism

For consistent rendering across runs:

- Pin Chromium version (Playwright manages this).
- Disable subpixel anti-aliasing if cross-platform consistency matters.
- Use `font-render-hinting=none`.
- Disable GPU.

---

## Common pitfalls

| Pitfall | Fix |
|---|---|
| Fonts not loaded before screenshot | Wait for `document.fonts.ready` |
| Network calls happening despite policy | Use `page.route` to block |
| Memory leak across many renders | Restart browser every N (e.g. 100) renders |
| Slow first render after launch | Warm up the browser with a dummy navigate |
| Inconsistent text rendering Mac vs Linux | Pin font-render-hinting; use same fonts |
| Headless mode shows different layout than visible | Use `--headless=new` or run in container |

---

## Performance tips

- Reuse `context` across slides if possible (faster, but watch for state leak).
- Pre-warm the browser process.
- Keep HTML payloads under 1MB per slide.
- Avoid 4K+ inline images unless required.
- Use `omitBackground: false` only when needed.

---

## Versioning

Pin Playwright + Chromium versions. Worker version (`renderer-v1.4.2`) includes the Playwright version it was built with. Upgrades are tested against the regression corpus before deploy.
