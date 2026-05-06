# Offer — `weaura-ai`

> Enterprise AI platform that grounds every answer in your infrastructure: your runbooks, your metrics, your logs, your deploy timeline. Self-hosted or SaaS. Read-only by default; write operations require human approval.

---

## What we sell

WeAura is a deployable AI platform for SRE / DevOps / Platform teams. It ingests your operational signals (metrics, logs, traces, events, deploys) and your operational knowledge (runbooks, ADRs, post-mortems, docs) into a single grounded system. Engineers ask natural-language questions; WeAura returns answers with cited sources, in seconds, without pivoting through five dashboards.

It is **not** a generic AI chat. It is **not** another observability dashboard. It is the *correlation layer* that knows your specific infrastructure.

## The job to be done

> "I need to know — right now — why a service is misbehaving, with sources I can verify, without having to context-switch through five tools, and without my data leaving our perimeter."

## The unique mechanism

**Three primitives compose the product:**

1. **Sinais correlacionados (Correlated signals).** Metrics, logs, traces, events, and deploy timeline ingested into a single causal graph. PromQL bridge with 14-day indexed retention. Loki + Tempo per-trace correlation. GitHub/GitLab deploy timeline cross-referenced with observed degradation.
2. **Knowledge base viva (Living knowledge base).** Runbooks, ADRs, post-mortems, docs (Confluence, Notion, Google Drive, GitHub markdown, PDFs) indexed with semantic re-ranking and tied to clusters. Every retrieved chunk is auditable.
3. **Citation contract.** Every response cites its sources. Every query / response / source is captured in an immutable audit trail. Configurable minimum citation threshold per tenant.

**Plus a safety mechanism that defines the product:**

- **Approval workflows (HITL gating).** Read-only operations (list pods, query metrics, view logs) execute immediately. Write operations (restart deployments, scale, apply IaC, change config) queue for human approval — with role-based gating, TTL, and full audit context.

## Core use cases

1. **Root-cause analysis during incidents.** "Why is `payment-api` restarting?" — answered in ~1.4s with metrics, deploy correlation, and runbook citation. (Site uses Marina's example: OOMKilled 3× in an hour, memory saturating GC since v2.3.0 deploy.)
2. **Operational knowledge retrieval.** "What's the runbook for our DB failover?" — exact runbook excerpts returned with semantic search.
3. **Onboarding new engineers.** New hires query the platform instead of asking the team. Tribal knowledge becomes addressable.
4. **Cross-tool correlation.** Connect deploy events, metric degradation, log anomalies, and runbook procedures in one query — instead of pivoting between Grafana, GitHub, Loki, Confluence, PagerDuty.

## Use cases we explicitly *don't* serve

- **Auto-remediation without human approval.** Write operations always go through approval workflow. We will not bypass this.
- **Generic Q&A.** "What's the weather?" / "Summarize this PDF unrelated to ops" → not the product.
- **Ungrounded brainstorming.** If the answer can't be sourced from your infra or your knowledge base, we don't fabricate one.
- **Long-form content generation.** WeAura answers ops questions. Not a writing tool.
- **Customer-facing chatbots.** Internal-ops only.

> Saying no to auto-remediation is the most expensive product decision WeAura has made. It will keep saying it.

---

## Pricing model

- **How we charge:** _(to be added — pricing not visible on the public site at the time of brand-pack creation)_
- **Public pricing tiers:** _(to be added)_
- **Anchor: how the audience tends to compare cost:** vs hiring an additional SRE; vs the cost of one P0 incident with delayed RCA; vs an internal RAG-over-Confluence project nobody maintains.

## Delivery model

- **SaaS** with multi-tenancy, OR
- **Self-hosted** via Helm chart with on-premises embeddings and zero data egress.
- **Data residency:** BR / US / EU / custom (per documented compliance posture).
- **Onboarding promise:** "Deploy in minutes, not weeks."

## Integrations (native)

- **Kubernetes:** EKS, GKE, AKS, on-premises (read-only service account).
- **Prometheus:** PromQL bridge, 14-day indexed retention, automatic alert correlation.
- **Loki / Tempo / OTEL:** structured logs, traces, metrics.
- **GitHub / GitLab:** commits, PRs, deploys correlated with degradation.
- **ArgoCD:** deploy timeline.
- **Confluence / Notion / Google Drive / GitHub markdown / PDFs:** knowledge ingestion.
- **PagerDuty / Slack:** bidirectional incidents + war-room bot.

## Identity & enterprise

- **SAML 2.0, OIDC, SCIM.** 2FA TOTP enforced for admins.
- **Identity provider:** integrates with Keycloak / customer SSO.
- **Multi-tenancy:** row-level security, tenantId carried implicitly on every query.

## LLM model strategy

- Pluggable: OpenAI, Anthropic, or open-source models via vLLM.
- **User data never trains models.**

---

## Differentiators (5)

1. **Grounding in *your* environment, not a generic model.** Every answer comes from *your* runbooks, *your* metrics, *your* deploy timeline. Generic AI assistants can't do this.
2. **Citation contract — every answer cites the source.** Configurable minimum threshold per tenant. Audit trail captures query + response + sources.
3. **HITL approval workflow built into the product.** Read-only auto-executes; writes require human sign-off. Role-based gating and TTL.
4. **Self-hosted with zero data egress.** Helm chart deploy. Embeddings on-premises. SOC 2 Type II, ISO 27001, GDPR. Data residency in BR / US / EU / custom.
5. **Native ops integrations, not generic webhooks.** Purpose-built connectors for K8s, Prometheus, Loki/Tempo, ArgoCD, GitHub, Confluence, PagerDuty.

---

## Alternatives the audience considers

### Generic AI chat assistants (e.g. category leaders we don't name in posts)
- **Type:** indirect; current default for engineers experimenting with AI.
- **Where they win:** open-ended Q&A, drafting, language tasks.
- **Where we win:** anything requiring *your* infrastructure context. Generic AI hallucinates; WeAura cites.

### Observability platforms with bolted-on AI features
- **Type:** adjacent. Same data, but chat-shaped.
- **Where they win:** if you already pay for that platform and want one more feature.
- **Where we win:** correlation across the *full* tool stack (deploy + observability + knowledge), not just within one vendor's silo.

### Internal RAG-over-Confluence projects
- **Type:** DIY alternative.
- **Where they win:** free; customizable.
- **Where we win:** maintained, integrated with live ops signals (not just static docs), enterprise compliance posture, semantic re-ranking, citation contract, approval workflow — all of which the average DIY project lacks.

### "Do nothing" — the spreadsheet of dashboards + the human war-room
- **Why audiences default here:** "We've always done it this way." The 5-dashboard pivot feels like operational competence, not a tax.
- **What it costs them:** measurable hours per week per engineer; un-measurable trust during P0 incidents.
- **The trigger that makes them act:** a P0 RCA that took 6+ hours to compile. Or a SOC 2 audit finding about runbook drift.

---

## Positioning statement

**WeAura is the AI platform for SRE / DevOps / Platform teams that want every answer grounded in their own infrastructure — with cited sources and human-approved write operations — without their data leaving their perimeter.**

Compressed: *"A IA que conhece sua infraestrutura."* / "The AI that knows your infrastructure."

---

## What we promise

1. **Every answer cites its source.** Configurable minimum threshold. No exceptions.
2. **Read-only operations execute in seconds.** No pivoting between five dashboards.
3. **Write operations always require human approval.** No auto-remediation surprises.
4. **Your data never trains the model.** Self-hosted is first-class.
5. **Deploy in minutes, not weeks.** Helm chart. Native Kubernetes. No professional-services lock-in.
