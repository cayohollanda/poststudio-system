# How to Use PostStudio System with Claude Projects

> Claude Projects let you upload a folder of files as persistent knowledge for a single, scoped chat. PostStudio System is built to be uploaded as one Project — and once it's there, you mostly just chat.

This guide is a companion to `docs/getting-started.md` and goes deeper on the Project setup, custom instructions, and recurring usage patterns.

---

## Why Claude Projects

Three reasons:

1. **Persistent knowledge.** The whole `poststudio-system/` repo lives in the Project. You don't paste it every time.
2. **Custom instructions.** The Project's instructions act as a permanent system prompt — Claude remembers it across every chat in that Project.
3. **Multi-thread.** You can spin up a new chat per carousel without losing context. The knowledge stays loaded.

A Claude Project is the closest thing to "running" PostStudio System that exists today.

---

## One-time setup

### 1. Create the Project

- Open Claude → Projects → New Project.
- Name it something like `[BRAND] Content Engine` if it's brand-specific, or `PostStudio System (multi-brand)` if you'll use it for multiple brands.

### 2. Upload Project Knowledge

Upload the entire `poststudio-system/` folder. Claude Projects support folders or zip uploads.

If your tool only allows individual files, prioritize uploading these in order:

1. `system/master-instructions.md` (mandatory)
2. `system/content-system.md`
3. `system/carousel-framework.md`
4. `system/visual-framework.md`
5. `system/copywriting-rules.md`
6. `system/safety-and-claims-rules.md`
7. `system/quality-checklist.md`
8. `prompts/generate-carousel.md`
9. `prompts/critique-carousel.md`
10. `brands/[your-brand-slug]/` (all 8 files)
11. `examples/example-carousel-output.md` (so Claude can see the output format)
12. `docs/output-contract.md`

If you have remaining capacity, add `prompts/generate-image-prompts.md`, `prompts/improve-hook.md`, `prompts/export-carousel-json.md`, and `schemas/carousel-output.schema.json`.

### 3. Paste custom instructions

In the Project's "Instructions" or "Custom instructions" field, paste:

```
You are operating PostStudio System.

Boot sequence (every conversation):
1. Read system/master-instructions.md as your top-level system prompt.
2. Read system/content-system.md, system/carousel-framework.md, system/visual-framework.md, system/copywriting-rules.md, system/safety-and-claims-rules.md, and system/quality-checklist.md.
3. When the user names a brand, treat brands/[brand-slug]/ as the active Brand Pack and read all 8 files.
4. If the user references a prompt by filename (e.g. "use generate-carousel"), load that prompt and follow its instructions.
5. Default to prompts/generate-carousel.md when the user asks for a carousel without specifying a prompt.

Output rules:
- Always end output with the Quality Checklist and Missing Inputs / Proof Needed sections.
- Never invent numbers, customers, awards, or quotes. Use [PROOF_NEEDED: short description] and surface it.
- Honor the active Brand Pack as a hard constraint (voice, words-to-avoid, claims-allowed, claims-forbidden).
- Match the output contract in docs/output-contract.md exactly.

Critique rules:
- When the user asks for a critique, switch to reviewer mode using prompts/critique-carousel.md.
- Do not approve work you wrote in the same response.

When the user is vague, ask up to 3 clarifying questions, then proceed and list assumptions explicitly.

Be confident, fast, and opinionated. Do not pad output. Do not apologize for taking a stance.
```

### 4. (Optional) Add an "active brand" pin

If you'll use the Project for one brand only, add this line at the top of the instructions:

```
The active brand for this Project is brands/[your-brand-slug]/. Load it on every conversation.
```

For multi-brand Projects, leave it out — you'll name the brand in each chat.

---

## Recurring usage patterns

### Pattern A — One-shot carousel

Single chat, one post out:

```
Use brands/[slug]/ and generate a carousel about "[topic]" for [platform], language [code], goal: [save/comment/DM/click].
```

Optionally append:

```
Use angle: [angle from content-system.md].
Use visual mode: [mode from visual-framework.md].
Use structure: [default / launch / educational / comparison / benchmark / thought-leadership / product-led / project-announcement].
```

### Pattern B — Brief → carousel → critique → revise

A quality-first 4-message sequence:

1. **You:** "Brief: [topic, audience, goal, constraints]. Use brands/[slug]/."
2. **Claude:** asks clarifying questions if needed, then generates the carousel.
3. **You:** "Run prompts/critique-carousel.md on this carousel. Be adversarial."
4. **Claude:** reviews and proposes the highest-leverage fix.
5. **You:** "Apply fix #1. Regenerate slide 1 and the affected slides only."
6. **Claude:** revises.

### Pattern C — Brand bootstrap

If the brand doesn't exist:

1. **You:** "Run prompts/generate-brand-pack.md."
2. **Claude:** asks the 10-section interview, one section at a time.
3. **You:** answer each section.
4. **Claude:** outputs the 8 Brand Pack files.
5. **You:** save them under `brands/[your-brand-slug]/` and re-upload to the Project.

### Pattern D — Series planning

For a content series:

```
Plan a 5-post carousel series for brands/[slug]/ on the theme of "[theme]".
For each post, give: angle, hook, slide structure (just the spine, not full slides), and dependency notes (which post should publish first).
```

Then generate them one at a time as you ship.

### Pattern E — Hook lab

When you want to test hooks fast:

```
Use prompts/improve-hook.md.
Topic: "[topic]".
Brand: brands/[slug]/.
Generate 12 hook variants, each tagged with the angle and the audience reaction it's optimizing for.
```

### Pattern F — JSON export

After a carousel is approved:

```
Run prompts/export-carousel-json.md on the carousel above.
Validate against schemas/carousel-output.schema.json.
```

---

## Multi-brand Project structure

If you manage multiple brands in one Project:

```
brands/
├── lumen/         (fictional example)
├── client-a/
├── client-b/
└── personal/
```

Custom instructions to add:

```
Active brand defaults to none. The user must name a brand at the start of each conversation.
If the user does not name a brand, ask which one before generating.
Never mix two brands' Pack data in one carousel.
```

This keeps Brand Packs isolated and prevents voice/claims bleed-through.

---

## Token / context strategy

Claude Projects with full PostStudio System loaded use a meaningful chunk of context. To stay efficient:

- Keep `system/` files lean. Don't pad them with examples that aren't load-bearing.
- Move long examples to `examples/` rather than `system/`.
- For one-off chats, you can reference rather than load: "Read system/master-instructions.md and brands/[slug]/voice.md only. Skip the rest unless needed."
- For automation pipelines (using the API, not Claude.ai), pass only the files needed for the specific prompt.

---

## What if you don't use Claude

The system is LLM-agnostic. For other tools:

- **OpenAI / GPT:** create a Custom GPT with the same files. Paste the custom instructions. Behavior is comparable.
- **Cursor / Copilot for Markdown:** open the repo in your editor, point the LLM at it, and reference files inline.
- **API workflows:** see `prompts/generate-carousel.md` for the canonical prompt structure. Pass `system/master-instructions.md` + the Brand Pack + the prompt as messages.

The output contract in `docs/output-contract.md` is portable. The schemas in `schemas/` make automation straightforward.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Output is generic | Brand Pack missing or under-specified | Run `prompts/generate-brand-pack.md` to enrich it. |
| Output uses forbidden words | Voice file's "words to avoid" not loaded | Re-upload `brands/[slug]/voice.md`. |
| Hooks are weak | Brief is vague | Add: angle + signal type (proof / opinion / story / process). |
| Output invents numbers | Safety rules not loaded | Re-upload `system/safety-and-claims-rules.md`. |
| Slides repeat | Structure not specified or default ignored | Specify a structure from `system/carousel-framework.md`. |
| Visual prompts are flat | Visual framework not loaded | Re-upload `system/visual-framework.md`. |
| Output skips quality checklist | Custom instructions not enforcing it | Re-paste custom instructions. |

---

## A note on "memory" between chats

Claude Projects do not (today) automatically learn from prior chats. If you want continuity:

- Capture insights in `brands/[slug]/proof-assets.md` and `examples.md`.
- Re-upload the brand folder when you've updated it.
- For long campaigns, keep a `brands/[slug]/changelog.md` (your private, not part of the system) so future chats can read what's worked.

The repo is your memory. The Project is your loader.
