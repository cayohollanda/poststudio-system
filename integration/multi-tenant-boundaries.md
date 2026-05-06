# Multi-Tenant Boundaries

> How to keep brands isolated. Cross-brand contamination is the #1 risk in a multi-tenant content platform.

---

## The principles

1. **One Brand Pack per Claude API call.** Never include two brands in a single prompt.
2. **Per-tenant API keys.** No shared credentials.
3. **Per-brand storage partitions.** No cross-brand database queries by default.
4. **Logs partitioned by `brand_id`.** Cross-brand log queries require explicit auth.
5. **Caching keyed by tenant + brand.** No global caches that blur brands.
6. **Rate limits scoped per tenant.** A loud tenant doesn't degrade another.

---

## What "contamination" looks like

- Brand A's voice tonality bleeds into Brand B's carousel because they shared a Claude session.
- Brand A's customer quote appears in Brand B's caption because the prompt loaded both packs.
- A reviewer accidentally references Brand A's accent color while reviewing Brand B's renderable.

All preventable. All catastrophic for trust.

---

## Storage isolation

```
tenants/
  tenant-A/
    brands/
      brand-1/
        brand-pack/
        outputs/
        feedback-log/
      brand-2/
        ...
  tenant-B/
    brands/
      ...
```

Database tables include `tenant_id` + `brand_id` as part of every primary key.

---

## Prompt assembly hygiene

When the CRUD platform assembles a prompt:

- Fetch Brand Pack from `tenant_id + brand_id` only.
- Validate that no other brand's data is included.
- Log the brand_id used, alongside the prompt_version and model.

```javascript
function assemblePrompt({ tenantId, brandId, module, brief }) {
  const brandPack = db.fetchBrandPack(tenantId, brandId);
  if (!brandPack || brandPack.tenantId !== tenantId) {
    throw new Error("Brand Pack not found or wrong tenant");
  }
  return [
    coreSystem,
    moduleInstructions(module),
    serializeBrandPack(brandPack),
    schemaReference(module),
  ].join("\n\n=== LAYER ===\n\n");
}
```

---

## Cross-brand queries

Some operations cross brands legitimately:

- Tenant-level analytics (aggregate, anonymized).
- Cross-tenant ops (impersonation by support; logged + audited).

These are *exceptions*. Default is strict isolation.

---

## Brand-pack secrets

Some Brand Pack fields are sensitive:

- `proof-assets.md` lists "things we cannot cite publicly."
- `constraints.md` lists trigger phrases for legal review.
- Internal customer names anonymized for public use.

The CRUD platform must treat these as the brand's secrets — not exposed to other brands' contexts, not surfaced in public logs.

---

## Multi-brand UI

When a single user manages multiple brands:

- Show explicit brand context in every screen.
- Confirm `brand_id` on every job submission.
- Visually distinguish brands (color tag, logo).
- Never auto-suggest copying content across brands without explicit user action.

---

## Defensive logging

Every Claude API call logs:

- `tenant_id`, `brand_id`.
- Prompt hash (so we can verify *which* assembled prompt ran).
- Model and `prompt_version`.
- Result hash.

Anomaly detection: if a Claude API call's prompt contains the string `brand_a_slug` AND `brand_b_slug` simultaneously, alert. That's a contamination event.

---

## When two brands need to share content

Rare but legitimate cases:

- Co-authored launch (parent brand + sub-brand).
- Press release with quotes from both brands.

For these:

- Treat as a *third Brand Pack* (the joint Brand Pack).
- Don't union the two packs in one prompt.
- Document the joint pack as an explicit artifact with both parties' approval.

---

## Audit trail

For every job:

- Who created it (`user_id`).
- For which tenant + brand.
- Which Brand Pack version was loaded.
- Which prompt_version ran.
- Who approved (if applicable).

Audit logs are immutable, retained ≥ 12 months.
