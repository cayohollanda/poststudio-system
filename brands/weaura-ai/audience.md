# Audience — `weaura-ai`

> Senior SRE / Platform / DevOps engineers running Kubernetes-based infrastructure at companies that take incidents seriously. They've stopped believing generic AI tools can help them — and they've also stopped believing more dashboards will.

---

## Primary audience

- **Role:** Senior SREs, Platform Engineers, DevOps engineers, and technical CTOs.
- **Situation:** Running Kubernetes (EKS / GKE / AKS / on-prem) in production. Already have Prometheus + Grafana + Loki/Tempo + GitHub/GitLab + PagerDuty + a documentation system (Confluence/Notion). Have lived through enough 3am incidents to be skeptical of any "AI productivity" pitch.
- **Where they are in the buying journey:** Mostly **problem-aware → solution-aware**. They've experienced root-cause-analysis pain firsthand. They've evaluated (and probably rejected) 1-3 generic AI assistants because the answers were ungrounded.

### Their #1 pain
> "It takes me 20 minutes to find out why `payment-api` is restarting because the answer lives across five dashboards, three runbooks, two Slack threads, and a deploy timeline — and the runbook hasn't been updated since the last reorg."

### Their #1 desire
A correlated answer in seconds — sourced, citing the runbook line, the metric, the deploy SHA, and the log lines — without the answer having to leave their perimeter.

### What they tried before
- **Generic AI chat assistants** — useful for one-off questions; useless for "why is X happening *in our cluster, right now*" because they have zero context.
- **Observability platform "AI features"** — bolted-on chat over the same data they already had to pivot through. Didn't reduce dashboards.
- **Internal RAG over Confluence** — built it, deployed it once, watched it go stale because nobody owned the index.
- **More runbooks** — wrote them, never re-read them, didn't help during the actual incident.

### Beliefs they hold that are *getting in their way*
- "We just need better dashboards." → False. The bottleneck is correlation, not visualization.
- "Generic AI plus our docs in a vector DB is good enough." → False. The grounding has to be live (metrics, logs, deploys *now*), not a stale snapshot.
- "AI will hallucinate write operations and break production." → True if there's no approval workflow. False if there is one. WeAura solves this with HITL gating.

### Beliefs they hold that we *agree with*
- Generic AI without grounding is a liability, not a tool.
- Every answer must cite its sources.
- Write operations need human approval. Always.
- Data should stay inside the customer's perimeter when policy demands it.
- The most expensive incidents are the ones where nobody can find the runbook in time.

### Where they hang out online
- Hacker News (read mode mostly).
- LinkedIn — comment threads under SRE / platform-engineering writers.
- Closed Slack / Discord communities for SRE leaders, platform engineers, K8s operators.
- X / Twitter — lurk on accounts of SRE / observability practitioners.
- Reddit: r/sre, r/kubernetes, r/devops.
- Conference circuit: KubeCon, SREcon, IncidentCon.

### What they read / watch / listen to
- *The Pragmatic Engineer* (Gergely Orosz).
- *Sysadmin1138* / *Charity Majors* writing on observability.
- The Honeycomb / Datadog / Grafana engineering blogs.
- *SRE Weekly* / *DevOps Weekly* / *KubeWeekly* newsletters.
- The Google SRE / SWE Books (reference material).
- Talks from KubeCon, SREcon, Velocity.

---

## Secondary audiences

### Segment 2 — Engineering leaders / VPs of Engineering
- **Role:** VP Engineering, Director of Platform, Director of SRE.
- **Situation:** Approves the buying decision. Cares about compliance posture (SOC 2, ISO 27001, GDPR), data residency, and team productivity metrics.
- **Why they matter:** Budget owner. SOC 2 / ISO certifications + self-hosted option exist *for them*.

### Segment 3 — Security / Compliance officers
- **Role:** CISO, Compliance Lead.
- **Situation:** Reviews data flow, audit trail, and key management before any new AI tool is approved.
- **Why they matter:** Veto power. Self-hosted + immutable audit + "user data never trains models" exists *for them*.

---

## Non-audience (explicit)

- **Junior developers / non-ops engineers.** WeAura is for people who already understand kubectl, PromQL, runbooks. Not a learning tool.
- **Companies not running Kubernetes.** The native integrations (K8s, Prometheus, Loki/Tempo, ArgoCD) define the addressable surface.
- **Solo / hobbyist engineers.** Enterprise compliance posture (SOC 2, SAML, audit retention) is overkill for individual use.
- **Companies that won't tolerate any AI in their ops loop.** WeAura is opinionated about HITL gating but it *is* AI; if AI is banned, this isn't the tool.
- **Teams looking for a generic chat assistant.** WeAura is purpose-built for ops; it's not a general-purpose copilot.

---

## How they speak

- **Vocabulary they actually use:** "root cause," "correlated signals," "observability," "incident response," "post-mortem," "blast radius," "SLO budget," "deploy timeline," "RCA," "p99," "OOMKilled," "control plane," "blast door," "the runbook," "runbook drift," "tribal knowledge," "kubectl," "context-switching," "the pager."
- **Vocabulary they hate:** "synergy," "supercharge," "transform," "AI-powered" (generic), "next-gen," "revolutionary," "disrupt," "leverage" (as a verb), "10x," "rockstar engineer," "ninja."
- **Reading level / formality:** Mid-formal. Comfortable with technical language. Allergic to marketing fog. Will read 800-word essays if dense.
- **Lowercase / proper case:** Mixed. Many lean lowercase on social. Code identifiers are precisely cased.
- **Are they ironic / earnest / both?** Both. Dry humor about pager fatigue and incident war stories; earnest about the work itself.

---

## Sample personas

### Persona A — "The platform lead at a Series-C SaaS"
Renata, 34, Platform Lead at a 350-person Series-C SaaS in Brazil. Manages a 6-person platform team. Runs EKS. Their last 3 P0 incidents had RCAs that took 4-8 hours each because correlating Datadog metrics with ArgoCD deploys with the runbook in Confluence was a manual archaeology job. She's tried two generic AI chat tools; both gave wrong answers about *her* infrastructure. She follows *The Pragmatic Engineer*. WeAura's "the answer cites the source — always" pitch is what makes her actually click "request demo."

### Persona B — "The staff SRE at a fintech"
Diego, 38, Staff SRE at a 1,200-person fintech. Compliance-heavy environment. Cannot ship anything that touches customer data without a CISO sign-off. The self-hosted option + SOC 2 Type II + ISO 27001 + immutable audit trail are what get him past the security review. He's the voice of "we need it self-hosted or it's not happening."

### Persona C — "The technical CTO of a 50-person scale-up"
Lila, 42, technical CTO at a 50-person Series-B startup. Wears the SRE hat herself half the week. Doesn't have time to context-switch between 5 dashboards. Her ICP is the "deploy in minutes, not weeks" promise — Helm chart, native K8s, no SaaS-only lock-in.
