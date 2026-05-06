# Security and Secrets

> Secrets handling, threat surface, and minimum security posture.

---

## Secrets inventory

| Secret | Where it lives | Rotation cadence |
|---|---|---|
| Anthropic API key | Server-side env / vault | Quarterly |
| Tenant API keys | DB (hashed) | On demand |
| Webhook signing secrets | DB (encrypted) | On demand |
| Worker shared secret | Server-side env / vault | Quarterly |
| Object storage credentials | Server-side env / vault | Quarterly |
| Database credentials | Server-side env / vault | Quarterly |
| Encryption keys | KMS | Annually (rotate; old keys retained for decryption) |

---

## Secrets rules

- Never log a secret.
- Never store a secret in a Brand Pack.
- Never include a secret in a Claude prompt.
- Never expose a secret in the consumer-facing API.
- Never commit a secret to git (use `.gitignore`, secret scanning).
- Never share a secret across tenants.

---

## Threat surface

### High risk

- **Cross-tenant data leak** via shared cache / shared prompt assembly.
- **Prompt injection** via Brand Pack content (a malicious brand setting up content that manipulates Claude's later behavior).
- **Webhook signature bypass** if signing is weak or absent.
- **Worker SSRF** via unsanitized renderable content (e.g. `<img src="http://internal-ip">`).
- **API key leakage** via verbose error responses.

### Mid risk

- **Rate limit abuse** by a single tenant draining quota.
- **Denial of wallet** (DoW) via prompt injection that triggers expensive Claude calls.
- **Renderable XSS** persisted in artifacts and rendered at view time.
- **Audit log tampering** (mitigation: append-only stores).

### Lower risk (still address)

- **Replay attacks** on webhooks (mitigation: timestamp + nonce in signature).
- **Insider misuse** (mitigation: RBAC + audit logs + access reviews).

---

## Prompt injection mitigations

Brand Pack content is *trusted* (the brand owner authored it). But the brand owner could be compromised or careless. Mitigations:

- Sanitize Brand Pack inputs at write time (strip control characters, prompt-injection patterns like "ignore previous instructions").
- Treat Brand Pack as data, not as system instructions, in the prompt assembly.
- Never let a Brand Pack override the safety / claims rules.
- Log assembled prompts hashed; sample for review.

---

## Renderable safety

The rendering worker MUST sanitize HTML/CSS/SVG before rendering:

- Strip `<script>` tags.
- Strip `on*=` attributes.
- Strip `javascript:` URIs.
- Strip remote URLs in `href`, `src`, `url()`, `xlink:href`.
- Validate `data:` URI sizes (prevent worker memory bloat).
- Run rendering in a sandboxed browser process.
- Network-isolate the rendering process (no outbound).

See [`rendering/safety-and-sanitization.md`](../rendering/safety-and-sanitization.md).

---

## Authentication

- Tenant API keys: hashed in DB; transmitted over HTTPS; verified per request.
- User → CRUD platform: standard session auth (OAuth, JWT, etc.).
- CRUD → Claude: API key in header.
- CRUD → Worker: mTLS or shared secret.
- Worker → Object storage: scoped credentials per render job (when possible).

---

## Authorization

- Tenant boundary enforced server-side on every request.
- RBAC for users within a tenant: owner / editor / reviewer / viewer.
- Admins can impersonate (logged + audited).

---

## Encryption

- TLS 1.2+ on all connections.
- Database encrypted at rest.
- Object storage encrypted at rest (server-side encryption).
- Secrets encrypted in vault (KMS-backed).

---

## Incident response

- Define what triggers a security incident:
  - Cross-tenant data exposure.
  - Auth bypass.
  - Secret leak.
  - Unsafe renderable executed.
- Have a runbook: detection → containment → eradication → recovery → post-mortem.
- Notify affected tenants per legal requirement.

---

## Compliance

Depending on tenant industry:

- GDPR (EU users / data) — DPAs, data residency, right-to-delete.
- SOC 2 — controls audit annually.
- HIPAA (rarely applicable to content marketing) — only if PHI is involved.
- ISO 27001 — for enterprise tenants.

The CRUD platform's compliance posture should match the most demanding tenant.

---

## Data residency

If tenants require data residency:

- Storage and processing in named regions.
- Per-tenant region selection.
- No cross-region replication without explicit consent.
- Document in the tenant agreement.

---

## What to never do

- Never use the Anthropic API key client-side.
- Never let a tenant inspect another tenant's logs.
- Never run rendering with full network access.
- Never store secrets in Brand Pack content.
- Never use the same shared secret across services.
- Never disable audit logging.
