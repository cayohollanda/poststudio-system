# Output Template — Brand Pack Builder

> What the module returns. The 12 files concatenated, in canonical order, ready for the user to copy into `brands/[slug]/`.

---

## Markdown form (Mode 1)

The output is a single message containing 12 file-blocks, in the order:

```
# Brand Pack — `<brand-slug>`

> Save these files to `brands/<brand-slug>/`. Each block is one file.

---

## File 1 — `brand.md`

```markdown
[contents of brand.md — see brands/_template/brand.md]
```

---

## File 2 — `audience.md`

```markdown
[contents of audience.md]
```

[... continue for all 12 files in canonical order ...]

---

# Setup instructions

1. Create folder: `brands/<brand-slug>/`
2. Save each block above as the file named in its header.
3. Re-upload the folder to your Claude Project.
4. Run `prompts/01-run-brand-diagnosis.md` to validate the pack.
5. Run `prompts/03-generate-content-strategy.md` to produce a 30-day content plan.

# Open questions / next steps

- [list any [MISSING_INPUT] gaps the user can fill]
- [list any [PROOF_NEEDED] items the user should validate]
- [recommend a refresh date — typically 90 days from now]
```

---

## JSON form (Mode 2)

Conforms to `schemas/brand-pack.schema.json`. Returns the 12 sections as nested objects. The CRUD platform stores them keyed by `brand_slug`, either as 12 separate documents or one composite blob.

```json
{
  "version": "1.0",
  "prompt_version": "v2.0.0",
  "brand_slug": "<slug>",
  "generated_at": "2026-05-06T00:00:00Z",
  "data": {
    "brand": { ... },
    "audience": { ... },
    "offer": { ... },
    "positioning": { ... },
    "voice": { ... },
    "visual_style": { ... },
    "proof_assets": { ... },
    "constraints": { ... },
    "content_pillars": { ... },
    "competitors": { ... },
    "examples": { ... },
    "rejected_examples": { ... }
  },
  "missing_inputs": [],
  "warnings": [],
  "errors": []
}
```

---

## Notes

- In Mode 1, generate everything inline — Claude in a Project can't write files directly.
- In Mode 2, return the structured JSON; the CRUD platform persists it.
- Always include the `Open questions / next steps` block for the user.
- If the interview was incomplete, mark the unfinished sections clearly and recommend a follow-up session.
- Do not invent placeholder claims to fill `proof-assets.md`. Mark gaps and surface them.
