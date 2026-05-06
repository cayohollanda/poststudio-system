# Negative Prompts

> What to exclude. Universal negatives + per-mode negatives.

---

## Universal negatives (always include)

```
no text, no logos, no watermark, no real brand names, no flat 2D illustration,
no AI-typical hand anatomy, no oversaturated gradient blobs, no stock-photo composition,
no awkward face anatomy, no inconsistent perspective, no random typography overlays,
no copyrighted brand assets, no plastic skin
```

If the brand bans people: append `no faces, no people, no hands`.

---

## Per-mode additional negatives

### Cinematic Dark
```
no neon glow, no cyberpunk RGB, no muddy mid-tones
```

### Editorial Minimal
```
no harsh shadows, no busy backgrounds, no decorative texture
```

### Futuristic Interface
```
no Iron Man HUD, no cliché holograms, no glitch effects, no lens flares
```

### Surreal Product Metaphor
```
no dollar sign / lightbulb / brain / rocket clichés, no cartoon style
```

### Premium Founder/Operator
```
no smiling stock-photo faces, no laptop-at-coffee-shop scenes, no business attire clichés
```

### Technical Blueprint
```
no realistic photography, no 3D render look, no clean digital lines (keep imperfection)
```

### Exploded Diagram
```
no cartoon style, no oversaturated colors, no busy backgrounds
```

### Meme-but-Premium
```
no Comic Sans aesthetic, no low-resolution textures, no overused meme faces
```

### Luxury Object
```
no glitter, no excess sparkle, no smoke effects, no catalog symmetry
```

### Abstract System
```
no generic gradient blobs, no random geometric noise, no "Web3" aesthetic
```

### AI Command Center
```
no Iron Man HUD, no fake screenshots with readable text, no green Matrix code
```

### Before/After Split Scene
```
no hard collage line, no obvious retouching, no stock-photo "after"
```

---

## Per-generator negatives

### Midjourney
- Use `--no` flag with comma-separated negatives.
- Example: `--no text, logo, watermark, hands, faces, plastic`.

### ChatGPT Images / DALL-E
- Express negatives in plain words inside the prompt.
- Example: "Do not include any text, logos, watermarks, or real brand names."

### Flux
- Use plain-word negatives at the end of the prompt.
- Long negative lists work; flux respects them.

### Leonardo
- Use the negative-prompt field in the UI.
- Same vocabulary as Midjourney.

### Ideogram
- Sometimes generates clean text; if so, don't suppress text — but suppress logos and brand names.

---

## When to relax negatives

- **Mode 8 (Meme-but-Premium):** allow one readable word if the meme requires it.
- **Mode 11 (AI Command Center):** allow abstract UI elements but suppress readable text.
- **Hero slide for a product launch:** allow the product to be the subject (with explicit "no fake brand logo" if the brand isn't yours).

---

## Common omissions that hurt output

- Forgetting `no faces` on brands that ban people. Generators default to people.
- Forgetting `no real brand names`. Generators may put plausible logos.
- Forgetting `no AI-typical hand anatomy`. Hands are the #1 giveaway.
- Forgetting `no flat 2D illustration`. Generators default to flat for "clean" looks.
- Forgetting `no readable text`. Generators try to write headlines themselves and produce garbage.

---

## Self-check

Every image prompt produced by this module includes:

- [ ] Universal negatives.
- [ ] Mode-specific negatives.
- [ ] Brand-Pack-banned visuals (from `visual-style.md`'s `do_nots`).
- [ ] Generator-specific syntax for negatives.

Without these, the generator drifts toward the default. The default is generic.
