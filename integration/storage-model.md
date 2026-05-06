# Storage Model

> What the CRUD platform stores and how it's organized.

---

## Top-level entities

| Entity | Purpose | Lifetime |
|---|---|---|
| Tenant | A customer of the platform | Indefinite |
| Brand | A brand within a tenant | Indefinite |
| Brand Pack | The 12-file context for a brand | Versioned per change |
| Briefing | A single content brief | Per job |
| Job | A generation + (optional) render run | Indefinite (with archival) |
| Output | The artifact produced by a job | Indefinite (with archival) |
| Asset | Final rendered PNG/JPG | Indefinite (with archival) |
| Webhook subscription | Per-tenant event subscriptions | Indefinite |
| Audit log | Immutable record of every action | ≥ 12 months |

---

## Brand Pack storage

Two valid models:

### Composite blob (simpler)

```sql
CREATE TABLE brand_packs (
  id UUID PRIMARY KEY,
  tenant_id UUID NOT NULL,
  brand_id UUID NOT NULL,
  version INT NOT NULL,
  data JSONB NOT NULL,  -- conforms to brand-pack.schema.json
  created_at TIMESTAMPTZ,
  created_by UUID,
  UNIQUE(brand_id, version)
);
```

### 12 separate documents (more flexible)

```sql
CREATE TABLE brand_pack_sections (
  id UUID PRIMARY KEY,
  tenant_id UUID NOT NULL,
  brand_id UUID NOT NULL,
  section_name TEXT NOT NULL,  -- 'brand', 'audience', 'offer', etc.
  version INT NOT NULL,
  content_md TEXT,
  content_json JSONB,
  created_at TIMESTAMPTZ,
  UNIQUE(brand_id, section_name, version)
);
```

Pick based on update granularity. If brand sections are rarely updated independently, composite is simpler. If sections are touched frequently and independently, separate documents avoid version churn.

---

## Job storage

```sql
CREATE TABLE jobs (
  id UUID PRIMARY KEY,
  tenant_id UUID NOT NULL,
  brand_id UUID NOT NULL,
  module TEXT NOT NULL,
  status TEXT NOT NULL,
  prompt_version TEXT NOT NULL,
  request JSONB NOT NULL,
  request_hash TEXT NOT NULL,
  created_at TIMESTAMPTZ,
  updated_at TIMESTAMPTZ,
  completed_at TIMESTAMPTZ,
  cost_usd NUMERIC(10,4),
  duration_ms INT,
  brand_pack_version INT NOT NULL
);

CREATE INDEX ix_jobs_status ON jobs(status);
CREATE INDEX ix_jobs_brand_status ON jobs(brand_id, status);
CREATE INDEX ix_jobs_created ON jobs(created_at);
```

---

## Artifact storage

Outputs (Markdown / JSON / renderable JSON / quality reviews) live in object storage. Database holds only references:

```sql
CREATE TABLE artifacts (
  id UUID PRIMARY KEY,
  job_id UUID NOT NULL,
  name TEXT NOT NULL,           -- 'carousel.json', 'quality-review.json', etc.
  type TEXT NOT NULL,           -- 'json', 'markdown', 'image'
  storage_url TEXT NOT NULL,    -- s3:// path
  size_bytes BIGINT,
  hash TEXT,
  created_at TIMESTAMPTZ,
  UNIQUE(job_id, name)
);
```

Object storage path:

```
s3://crud-bucket/tenants/{tenant_id}/brands/{brand_id}/jobs/{job_id}/{artifact_name}
```

---

## Asset storage (rendered PNGs)

```sql
CREATE TABLE rendered_assets (
  id UUID PRIMARY KEY,
  job_id UUID NOT NULL,
  slide_number INT NOT NULL,
  storage_url TEXT NOT NULL,
  width INT, height INT,
  size_bytes BIGINT,
  format TEXT,                  -- 'png', 'jpg'
  created_at TIMESTAMPTZ,
  UNIQUE(job_id, slide_number)
);
```

Asset path:

```
s3://crud-bucket/tenants/{tenant_id}/brands/{brand_id}/jobs/{job_id}/assets/slide-{n}.png
```

---

## Audit log

```sql
CREATE TABLE audit_log (
  id UUID PRIMARY KEY,
  tenant_id UUID,
  brand_id UUID,
  user_id UUID,
  action TEXT NOT NULL,         -- 'job.created', 'brand_pack.updated', 'output.approved'
  target_id UUID,
  metadata JSONB,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX ix_audit_tenant ON audit_log(tenant_id, created_at);
```

Append-only. No updates. No deletes within retention window.

---

## Retention

| Data | Active | Archived | Hard-deleted |
|---|---|---|---|
| Brand Pack | Indefinite | n/a | On tenant offboarding |
| Jobs | 90 days | 90d-2y (cold) | 2y |
| Artifacts | 90 days | 90d-2y (cold) | 2y |
| Rendered assets | 90 days | 90d-2y (cold) | 2y |
| Audit log | 12+ months | 12m-7y (cold) | 7y |

Adjust per legal / compliance needs.

---

## Backup & recovery

- Daily snapshot of all databases.
- Point-in-time recovery for the last 7 days.
- Object storage versioning enabled (so accidental overwrites recoverable).
- Quarterly restore drills.
