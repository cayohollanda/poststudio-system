# Module — Agency Delivery Kit

> Reusable delivery process for agencies, freelance content teams, and internal brand departments. Onboarding → discovery → input → delivery → review → handoff → reporting.

---

## What it is

A complete operational kit for productizing PostStudio System as a service. Think of it as the "agency wrapper" around the content modules.

## Who it's for

- Content agencies running multi-client carousel / reels / campaign delivery.
- Freelance ghostwriters and content strategists.
- Internal brand teams managing 3+ product lines or sub-brands.
- Consultancies offering monthly content as a productized engagement.

## What it's NOT

- A specific company's branded service.
- A SaaS product.
- A legal contract template.

It's the *workflow*. Adapt the language to your operation.

---

## Files in this module

| File | Purpose |
|---|---|
| `README.md` | This file |
| `client-onboarding.md` | First-week onboarding flow |
| `discovery-questionnaire.md` | Brand discovery questions |
| `access-request-checklist.md` | What you need from the client to start |
| `input-collection-checklist.md` | Recurring input collection per cycle |
| `delivery-workflow.md` | The end-to-end delivery process |
| `review-workflow.md` | Internal QA + client review flow |
| `handoff-template.md` | What to deliver and how |
| `monthly-content-plan-template.md` | The monthly plan deliverable |
| `reporting-template.md` | Monthly reporting / retrospective |
| `scope-boundaries.md` | What's in scope, what's out, how to say no |
| `revision-rules.md` | Revision policy + change-request handling |
| `client-feedback-form.md` | Structured feedback capture |

---

## Mode support

| Mode | Supported? |
|---|---|
| Claude Project Manual | Yes — primary use |
| API Production | Indirect — the CRUD platform may implement the workflow |
| Renderable Creative Worker | n/a |

---

## How to use the kit

1. **First engagement:** run `client-onboarding.md` + `access-request-checklist.md` + `discovery-questionnaire.md`.
2. **First content cycle:** use `input-collection-checklist.md` to gather monthly inputs, then run `delivery-workflow.md`.
3. **Per deliverable:** internal QA with `review-workflow.md`, then `handoff-template.md` to client.
4. **End of month:** generate `monthly-content-plan-template.md` for the next cycle and run `reporting-template.md` on the past cycle.
5. **Edge cases:** revisions follow `revision-rules.md`; out-of-scope requests follow `scope-boundaries.md`.

---

## Related

- All content modules: this kit assumes you have the carousel-machine, reels-script-machine, campaign-builder, brand-pack-builder, brand-diagnosis-agent, content-strategy-os, quality-gate already running.
- Prompt: [`prompts/11-create-client-delivery.md`](../../prompts/11-create-client-delivery.md), [`prompts/12-create-monthly-content-plan.md`](../../prompts/12-create-monthly-content-plan.md)
