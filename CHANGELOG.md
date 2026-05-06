# Changelog

All notable changes to PostStudio System will be documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [2.1.1] — 2026-05-06

### Mode 9 lock — `decoded-editorial` is the mandatory Mode 1 visual layout

Field test of v2.1.0 surfaced a real failure: `prompts/19-ephemeral-brand-intake.md` was instructing Claude to **infer** a visual mode by inspecting the brand's reference posts. When a user attached the brand's pre-existing editorial posts as visual references, Claude correctly read them as cues — but then adopted that pre-existing layout as the Mode 1 template, instead of applying the brandsdecoded canonical layout (Mode 9). The result: every brand got its own visual identity reflected back, defeating the whole point of having a system-level signature template.

This release locks `decoded-editorial` (Mode 9) as the canonical, mandatory visual layout for every Mode 1 carousel. Brands customize only the 13 placeholder values; reference posts are color/typography cues only.

### Changed

- **`system/master-instructions.md`** — added operating principle #15: "Visual mode in Mode 1 is LOCKED to Mode 9 `decoded-editorial`." Brand pack supplies only the 13 placeholders, never an alternative layout. Modes 1-8 documented for Mode 2/3 explicit overrides only.
- **`system/visual-framework.md` Mode 9** — re-headlined as "CANONICAL DEFAULT FOR MODE 1." Removed "use this for long-form ≥10 slides" qualifier; the layout scales to short carousels too. Added explicit risk: "copying layout from the brand's existing reference posts." Updated mode-selection filter: Mode 1 has no choice; Modes 2/3 honor the structured request.
- **`prompts/19-ephemeral-brand-intake.md`** — Visual mode is no longer a user input. Section 06_visual-style.md output MUST declare `primary_visual_mode: decoded-editorial` and fill the 13 placeholders. Reference posts framed as color/typography cue sources only — explicit hard rule: "Do NOT extract layout, grid, header chrome, or slide composition from the brand's own existing posts. The brandsdecoded template defines the layout. Period."
- **`prompts/04-generate-carousel.md`** — Visual mode input replaced with the lock notice. Process step 4 rewritten: "Visual mode is LOCKED to Mode 9 in Mode 1. Pull the 13 placeholder values from the brand pack."
- **`prompts/18-deliver-carousel-as-png-zip.md`** — Layout rules section now opens with the Mode 9 lock declaration and the warning to never adapt rendered layout to mimic brand reference posts.

### Migration notes from v2.1.0

If you ran intake under v2.1.0 and got a different visual mode (e.g. `editorial-minimal`), re-run the intake under v2.1.1. The brand pack will now correctly declare `primary_visual_mode: decoded-editorial` with the 13 placeholders filled from the same source material. Output will follow the brandsdecoded canonical layout regardless of what the brand's pre-existing posts looked like.

---

## [2.3.0] — 2026-05-06

### PostStudio Machine — modo entrevista Socrática + fact-check ativo

This release replaces the briefing-em-rajada da PostStudio Machine pelo **modo entrevista Socrática**: uma pergunta por turno, confirmação de paráfrase obrigatória, detecção de vagueza com reformulação automática, e web search ativo na Etapa de Evidências. O fluxo total agora leva ~30-40 minutos (~25-35 mensagens) e produz output significativamente mais denso e brand-fluent.

### Added

- **`system/machine/07-interview-protocol.md`** — protocolo Socrático completo. Define princípios (uma pergunta por turno, confirmar paráfrase, não aceitar vagueza, adaptar próxima pergunta, reformular quando o usuário foge), heurísticas de detecção de vagueza (teste da substituição, buzzword check, audience check, insumo check), 5 etapas com perguntas-âncora + perguntas-de-aprofundamento (Insumo & Tensão, Brand & Voz, Ângulo Editorial, Evidências com Web Search Ativo, Confirmação Final), anti-patterns de entrevista, cadência ideal (~18-26 turnos só na entrevista, +8-12 no pipeline).

### Changed

- **`prompts/20-poststudio-machine.md`** — refatorado por completo. Substitui o Briefing Criativo (rajada de 7 perguntas) pelo modo entrevista Socrática. Boot agora carrega 11 arquivos em vez de 10 (inclui `07-interview-protocol.md`). Ponto de entrada vai direto pra Pergunta 1 da Etapa 1 (Insumo). Bloco 3 redefinido como "Modo Entrevista Socrática (regras obrigatórias)" com 6 mandamentos. Cada etapa tem perguntas-âncora explícitas, perguntas-de-aprofundamento adaptativas, e confirmação de paráfrase obrigatória.

- **`system/machine/02-editorial-quality.md`** — Parâmetro 4 (Fatos Verificados) elevado pra **Fact-Check Ativo**. Quando há web search disponível: rodar antes da Espinha Dorsal, confirmar todas as entidades, apresentar lista ✅/⚠️/🔎/❌ pro usuário decidir. Quando não: marcar `[PROOF_NEEDED]`. Princípios reforçados: nunca inventar, nunca arredondar sem dizer, datas obrigatórias, citações verbatim.

- **`system/machine/README.md`** — atualizado pra incluir `07-interview-protocol.md` (anatomia agora tem 7 arquivos). Seção "Como Claude usa" reescrita pra refletir o fluxo bimodal entrevista → pipeline.

- **`CHANGELOG.md`** — entrada v2.3.0.

### Why

A versão anterior (v2.2.0) usava briefing em rajada — Claude pedia 7 perguntas de uma vez, igual ao Content Machine v4 do brandsdecoded. Funciona pra usuário experiente que sabe o briefing de cor, mas:

- Pra brand nova / cliente novo / brand sem clareza: usuário responde vago e o output reflete a vagueza.
- Sem fact-check ativo: dados inventados ou imprecisos passavam silenciosos, Claude marcava `[PROOF_NEEDED]` ao invés de validar quando podia.
- Sem confirmação de paráfrase: divergência de entendimento entre Claude e usuário só aparecia no Texto Final (tarde demais).

O modo entrevista resolve os três problemas. Trade-off: leva ~2x mais tempo. Quem está com pressa usa `prompts/04-generate-carousel.md` em vez disso.

### Architecture

- **Substituição, não adição.** Não criamos prompt 21 — o prompt 20 agora é puramente Socrático. Pra fluxo rápido sem entrevista, o usuário muda pra prompt 04.
- **Compatibilidade preservada.** Quem rodou `prompts/19-ephemeral-brand-intake.md` antes do prompt 20 na mesma sessão tem a Etapa 2 (Brand & Voz) automaticamente reduzida a uma confirmação dos 10 campos do brand pack.
- **Web search é runtime.** Mode 1 (Claude.ai) tem ferramenta disponível por default. Mode 2/3 herda do client. Sem ferramenta = degrada graciosamente pra `[PROOF_NEEDED]`.

### Migration notes from v2.2.0

- Quem usava prompt 20 anterior pra fluxo rápido: agora abre 2 caminhos:
  1. Manter prompt 20 e usar o modo entrevista (recomendado pra qualidade)
  2. Mudar pra `prompts/04-generate-carousel.md` (briefing em rajada Markdown) + `prompts/18-deliver-carousel-as-png-zip.md` (export PNG)
- Brand pack ephemeral via prompt 19 continua funcionando — pula Etapa 2 da entrevista.
- Quality gate (`02-editorial-quality.md`) Parâmetro 4 ficou mais estrito — copy com dado errado encontrado via web search agora reprova o bloco inteiro até o usuário decidir.

---

## [2.2.0] — 2026-05-06

### PostStudio Machine — guided end-to-end carousel pipeline

This release adds the **PostStudio Machine** — an opinionated, guided flow for producing carousels from raw insumo to PNG zip in a single conversation. The Machine wraps and orchestrates briefing → headline engine → editorial spine → 7-parameter quality validation → text approval → image collection → HTML preview → Playwright PNG export.

The Machine is brand-agnostic by design: brand context is supplied per session via Briefing Criativo (or inherited from `prompts/19-ephemeral-brand-intake.md`). The framework defines structure, rules, and editorial quality bars; brand variables (color, fonts, handle, CTA pattern) are runtime data.

### Added

- **`prompts/20-poststudio-machine.md`** — system prompt for the PostStudio Machine. The full guided flow in a single prompt, mapping every etapa (Ponto de Entrada → Briefing Criativo → Triagem → 10 Headlines → Espinha Dorsal → Validação Editorial Invisível → Aprovação de Texto → Sugestão de Imagens → HTML Preview → PNG Export → Legenda). Includes nicho-based palette suggestions (12 nichos), font pairing by visual style (Clássico / Moderno / Minimalista / Bold), narrative arc adaptation by carousel type (Tendência / Tese / Case / Previsão), and slide rhythm adaptation (5/7/9/12 slides).
- **`system/machine/`** — núcleo editorial+visual da Machine, 6 arquivos:
  - `README.md` — orientação do subsistema
  - `01-headline-engine.md` — engine de geração e validação de headlines: padrões de lift (Brasil +155%, Fim/Morte +119%, Geracional +119%, Novidade +99%), 13 gatilhos emocionais, 4 estruturas de hook (Investigação Cultural, Narrativa Magnética, Dois-Pontos, Pergunta Geracional), 3 dimensões de avaliação obrigatória, banco de hooks de referência por padrão
  - `02-editorial-quality.md` — manual editorial com 7 parâmetros (Gramática, Fluidez, AI Slop, Fatos, Estrutura, Densidade, Tom Editorial). Nota mínima 8/10 em cada. Bloco abaixo de 8 reprova. 5 testes finais (Folha, Substituição, Promessa, Artigo, Binário). Estrutura obrigatória 18 blocos / 9 slides com adaptações pra 5/7/12.
  - `03-anti-slop-filter.md` — filtro anti-AI-slop completo: ~70 construções proibidas (paralelismos, headlines genéricas, vocabulário corporativo, anglicismos numéricos, aberturas/fechamentos preguiçosos), teste do tom de IA, regras gramaticais, checklist rápido.
  - `04-design-principles.md` — princípios de design: hierarquia de 3 níveis, ritmo dark/light/gradient, regra do terço inferior, escala tipográfica fixa, componentes (card / tabela / big stat / image-box / arrow rows / pattern card), geração algorítmica de paleta a partir de cor primária, anti-patterns visuais.
  - `05-template-alternado.md` — especificação visual completa do template canônico (Alternado Claro/Escuro): variáveis CSS, accent bar, brand bar (`POWERED BY POSTSTUDIO`), progress bar, slide capa (gradient pesado + badge handle), slides dark/light/gradient, image box, slide CTA com frase-ponte + keyword box, instagram preview frame, sequências de 5/7/9/12 slides, regras de embed de fontes via base64 `@font-face`, script Playwright de export.
  - `06-references.md` — 3 carrosséis completos como gabarito de qualidade (Tese Contraintuitiva sobre temas virais, Tendência Interpretada sobre cultura de corrida, Case/Benchmark sobre auditoria de cloud) com triagem → headline → espinha dorsal → copy de cada slide. Padrões de qualidade extraídos pra auto-validação.

- **Visual Framework Mode 10 — Alternado Claro/Escuro** adicionado a `system/visual-framework.md`. Esta é a modalidade canônica da PostStudio Machine. 13 placeholders documentados, regras de chrome universal, sequências adaptadas, integração com Mode 9 e demais modes.

### Changed

- **`prompts/00-start-here.md`** — adicionado prompt 20 (PostStudio Machine) à tabela de roteamento de tasks
- **`system/visual-framework.md`** — atualizado de 9 para 10 modes; tabela e árvore de decisão expandidas pra incluir Mode 10
- **`CHANGELOG.md`** — entrada v2.2.0

### Architecture

- A Machine é Mode 1 (Claude Project Manual) por design. Em Mode 2/3, a equivalente é a sequência `prompts/13-generate-renderable-carousel.md` + `prompts/17-export-render-job.md`.
- Os 6 arquivos de `system/machine/` são carregados apenas quando o usuário invoca o prompt 20 — não inflam contexto em sessões normais.
- A metodologia editorial (lift de headlines, 7 parâmetros, anti-AI-slop) foi adaptada de um system prompt de carrosséis virais com performance comprovada (referência creditada em `system/machine/README.md`).

### Migration notes from v2.1.0

- Existing prompts (00-19) e modos (1/2/3) continuam funcionando inalterados. O prompt 20 é aditivo — escolha quando usar:
  - Quer só copy em Markdown → `prompts/04-generate-carousel.md`
  - Quer só PNG zip de copy aprovada → `prompts/18-deliver-carousel-as-png-zip.md`
  - Quer pipeline completo guiado com QA editorial → `prompts/20-poststudio-machine.md` (NOVO)
- Brand pack ephemeral (`prompts/19-ephemeral-brand-intake.md`) continua sendo o caminho default pra brand context. Se rodado antes do prompt 20 na mesma sessão, o Briefing Criativo da Machine é pulado.

---

## [2.1.0] — 2026-05-06

### Brand-agnostic refactor — framework defines structure; brand instances are runtime data

This release enforces the philosophical principle that PostStudio System is brand-agnostic. The framework repo no longer contains real brand instances — only the `_template/` scaffold and the fictional `examples/example-brand-pack/` (Lumen). Brand context is now supplied per session via runtime intake (website + handle + voice samples + uploaded logos) or via a private `brands/[slug]/` folder uploaded directly into Project Knowledge.

### Added

- **`prompts/19-ephemeral-brand-intake.md`** — the canonical default brand-loading path. Consolidates per-session inputs (pasted website, handle, voice samples, attached logo files) into a 12-section in-memory brand pack. No commit, no save.
- **`system/visual-framework.md` Mode 9 — Decoded Editorial.** Promoted from a brand-specific spec into a system-level template with 13 placeholder slots. Any brand can adopt it by setting `primary_visual_mode: decoded-editorial` in `visual-style.md` and supplying placeholder values (accent, dark/light backgrounds, ink colors, fonts, mark file, handle, header labels, CTA pattern).
- **`examples/example-brand-pack/README.md`** — explicit note that Lumen is fictional and exists only as a structural reference.
- **`.gitignore`** — `brands/*` is now blocked with exceptions for `_template/` and `_template/**`. Prevents accidental commits of real brand instances.

### Changed

- **`system/master-instructions.md`** — boot-sequence step 3 inverted. Default brand loading is now ephemeral / session-supplied, not "load `brands/[slug]/` from the framework repo." Real brand instances do not live in this repo.
- **`prompts/00-start-here.md`, `04-generate-carousel.md`, `13-generate-renderable-carousel.md`** — brand-context inputs reflect ephemeral-first flow. Prompts 18 and 19 added to the routing table.
- **`prompts/18-deliver-carousel-as-png-zip.md`** — removed all literal references to a specific brand. Now uses generic `[BRAND]`/`[BRAND_SLUG]` placeholders and points to the brand pack's declared visual mode (typically Mode 9 `decoded-editorial`).
- **`README.md`, `docs/getting-started.md`, `docs/how-to-use-with-claude-projects.md`, `docs/workflow.md`** — onboarding flows now describe the two canonical paths: (A) ephemeral session intake via prompt 19, (B) private brand pack uploaded into Project Knowledge. Stale "8 files" references corrected to 12.
- **`prompts/02-generate-brand-pack.md`** — output now returns all 12 brand pack files (was 8). Save-to-disk instructions clarified: save outside this framework repo, upload privately into Project Knowledge.

### Removed

- Any brand instance previously committed to `brands/`. The framework's `brands/` folder now contains only `_template/`.

### Migration notes from v2.0.0

- If you had a private brand instance committed in your fork, move it outside the repo (e.g. to `~/Documents/Brands/[slug]/` or your own private repo). The new `.gitignore` will block re-commits.
- For new brand onboarding, prefer `prompts/19-ephemeral-brand-intake.md` over `prompts/02-generate-brand-pack.md` unless you want a full interview pass.
- Existing carousel outputs and renderable schemas are unchanged — this release does not touch generation contracts.

---

## [2.0.0] — 2026-05-06

### Major release — Full Content Operating System with three runtime modes

The repository is now a complete **Content Operating System** that operates in three runtime modes: Claude Project Manual Mode, API Production Mode, and Renderable Creative Worker Mode.

### Added

#### Three runtime modes
- **Mode 1 — Claude Project Manual:** humans chat with Claude in a Claude Project; Markdown output.
- **Mode 2 — API Production:** an external CRUD platform calls Claude API; JSON output, schema-validated.
- **Mode 3 — Renderable Creative Worker:** Claude → renderable HTML/CSS/SVG/template JSON → worker → final PNG/JPG slides.

#### `system/` (the OS kernel) — expanded
- `operating-system.md` — the master 12-stage lifecycle with per-mode behavior.
- `renderable-creative-framework.md` — universal rules for HTML/CSS/SVG/template-driven output.
- `reels-framework.md` — short-video script structures (15s / 30s / 45s / 60s / 90s).
- `campaign-framework.md` — 15 campaign types + 5-act arc + 7 / 14 / 30-day templates.
- `diagnosis-framework.md` — 12-category brand audit method.
- `output-rules.md` — Markdown output discipline.
- `api-output-rules.md` — JSON output discipline (Modes 2/3).
- `integration-contract.md` — the 3-role contract (CRUD / Claude / Worker).

#### `modules/` — 11 functional modules
- `brand-diagnosis-agent/` — 12-category audit.
- `brand-pack-builder/` — interview + context-mode pack creation.
- `content-strategy-os/` — pillars + series + 30/90-day plans.
- `carousel-machine/` — workhorse carousel generation (existed; expanded).
- `visual-prompt-system/` — 12 visual modes + image-prompt templates.
- `renderable-creative-system/` — HTML/CSS/SVG/template-driven output.
- `copy-vault/` — reusable hooks, headlines, CTAs, captions, arguments, objections, transitions, proof copy, launch copy, contrarian copy.
- `reels-script-machine/` — short-video scripts.
- `campaign-builder/` — 15 campaign types.
- `quality-gate/` — adversarial reviewer with 30-point + 10-point rubrics.
- `agency-delivery-kit/` — onboarding, discovery, handoff, reporting, scope, revisions, feedback.

#### `prompts/` — renumbered to 18 prompts (00-17)
- `00-start-here.md` — orientation.
- `01-run-brand-diagnosis.md` (new).
- `02-generate-brand-pack.md` (renamed).
- `03-generate-content-strategy.md` (new).
- `04-generate-carousel.md` (renamed).
- `05-generate-image-prompts.md` (renamed).
- `06-generate-reels-script.md` (new).
- `07-generate-campaign.md` (new).
- `08-critique-output.md` (renamed and generalized).
- `09-improve-output.md` (new — generalized improve loop).
- `10-export-json.md` (renamed and generalized).
- `11-create-client-delivery.md` (new).
- `12-create-monthly-content-plan.md` (new).
- `13-generate-renderable-carousel.md` (new — renderable output).
- `14-generate-html-css-slide.md` (new).
- `15-generate-svg-slide.md` (new).
- `16-repair-renderable-output.md` (new).
- `17-export-render-job.md` (new).

The previous `improve-hook.md` and `generate-visual-style.md` are kept alongside the numbered sequence for hook-specific iteration and visual-style bootstrap.

#### `schemas/` — expanded
- `brand-diagnosis.schema.json` (new).
- `content-strategy.schema.json` (new).
- `reels-script.schema.json` (new).
- `campaign.schema.json` (new).
- `quality-review.schema.json` (new).
- `monthly-content-plan.schema.json` (new).
- `renderable-carousel.schema.json` (new).
- `renderable-slide.schema.json` (new).
- `render-job.schema.json` (new).
- `render-result.schema.json` (new).
- `api-request.schema.json` and `api-response.schema.json` (new — aliases for the integration generation schemas).
- `generation-request.schema.json` and `generation-response.schema.json` (new — top-level mirrors).

#### `integration/` — new layer
- `production-architecture.md`, `claude-api-mode.md`, `context-loading-strategy.md`, `prompt-assembly-strategy.md`, `async-job-lifecycle.md`, `external-crud-platform-contract.md`, `partner-creative-api.md`, `webhook-contract.md`, `failure-and-retry-strategy.md`, `multi-tenant-boundaries.md`, `storage-model.md`, `observability-and-logging.md`, `security-and-secrets.md`.
- `integration/schemas/` with 6 JSON schemas: `generation-request`, `generation-response`, `creative-render-request`, `creative-render-response`, `job-status`, `webhook-event`.
- `integration/examples/` with 5 canonical request/response examples.
- `integration/playbooks/` with 5 runbooks: `generate-carousel-end-to-end`, `run-quality-gate-before-render`, `handle-missing-proof`, `retry-low-quality-output`, `onboard-new-brand-production`.

#### `rendering/` — new layer
- `architecture.md`, `renderable-output-contract.md`, `html-css-generation-rules.md`, `svg-generation-rules.md`, `safety-and-sanitization.md`, `playwright-renderer.md`, `sharp-optimization.md`, `template-vs-freeform-rendering.md`, `font-and-asset-handling.md`, `slide-dimensions.md`, `failure-and-retry-strategy.md`, `quality-assurance.md`.
- `rendering/schemas/` mirroring the renderable JSON schemas.
- `rendering/prompts/` mirroring the renderable Claude prompts.
- `rendering/examples/` with `renderable-carousel.json`, `html-css-slide.html`, `svg-slide.svg`, `render-job.json`, `render-result.json`.
- `rendering/playbooks/` with 5 runbooks: `end-to-end-rendering-workflow`, `mvp-html-to-png-workflow`, `svg-to-png-workflow`, `template-driven-rendering-workflow`, `debug-render-failures`.

#### `brands/_template/` — expanded to 12 files
- Added `positioning.md`, `content-pillars.md`, `competitors.md`, `rejected-examples.md`.

#### `outputs/_template/` — new
- 10 scaffold templates for downstream tooling: `carousel.md`, `diagnosis.md`, `brand-pack.md`, `content-strategy.md`, `reels-script.md`, `campaign.md`, `quality-review.md`, `monthly-content-plan.md`, `renderable-carousel.json`, `render-job.json`.

#### `docs/` — expanded
- `operating-model.md` — the philosophical layer behind the lifecycle.
- `module-overview.md` — map of the 11 modules + integration + rendering layers.
- `claims-and-proof.md` — the philosophical layer for safety rules.
- `api-production-mode.md` — Mode 2 guide.
- `renderable-output-mode.md` — Mode 3 guide.
- `agency-delivery-workflow.md` — high-level walk-through of the agency kit.
- `glossary.md` — terms used across the repo.

#### `examples/` — expanded
- `example-brand-pack/` (Lumen) extended to 12 files.
- `example-brand-diagnosis.md` (new).
- `example-content-strategy.md` (new).
- `example-reels-script.md` (new).
- `example-campaign.md` (new).
- `example-quality-review.md` (new).
- `example-monthly-content-plan.md` (new).
- Pointer files for renderable carousel / render job / render result examples.

#### `playbooks/` — new top-level folder
- `single-carousel-workflow.md`.
- `monthly-content-engine-workflow.md`.
- `brand-onboarding-workflow.md`.
- `launch-campaign-workflow.md`.
- `client-delivery-workflow.md`.
- `internal-brand-workflow.md`.
- `api-production-workflow.md`.
- `renderable-carousel-production-workflow.md`.

### Changed

- `system/master-instructions.md` — rewritten to orient Claude across all three modes, route to all 11 modules, and reference the 18 prompts.
- `README.md` — rewritten to describe the full Content OS, the three modes, the 11 modules, and the integration / rendering layers.
- `system/quality-checklist.md` — extended with renderability checks for Mode 3.
- `examples/example-brand-pack/` updated to 12 files (added the 4 new sections).

### Migration notes from v1.x

- Existing carousel outputs from v1.x continue to validate against `schemas/carousel-output.schema.json` (no breaking changes to that schema).
- Brand Packs from v1.x now require 4 additional files (`positioning.md`, `content-pillars.md`, `competitors.md`, `rejected-examples.md`); the Brand Pack Builder generates them.
- Prompts renumbered: previous `generate-carousel.md` is now `04-generate-carousel.md`. Update references in custom Claude Project instructions.
- Mode 2 / Mode 3 are net-new; v1.x users can keep operating in Mode 1.

### Design notes

- 100% brand-agnostic. Lumen example brand is fictional. Any resemblance to real companies is coincidental.
- Three runtime modes use the same repo files. Schemas, prompts, and Brand Packs travel between modes unchanged.
- Quality Gate runs as a separate Claude API call in Modes 2/3. Never the same context as generation.
- Rendering worker is stateless. Asset and font registries are managed by the worker, not by Claude.

---

## [1.0.0] — 2026-05-05

### Added — Initial public release

- Universal `system/` rules for hooks, slides, visual modes, copy, safety, quality.
- 7 prompts (`generate-carousel`, `generate-brand-pack`, `generate-visual-style`, `generate-image-prompts`, `critique-carousel`, `improve-hook`, `export-carousel-json`).
- 3 JSON schemas (`brand-pack`, `carousel-output`, `visual-style`).
- 8-file Brand Pack template + worked Lumen example.
- 6 docs (`getting-started`, `how-to-use-with-claude-projects`, `workflow`, `content-principles`, `visual-principles`, `output-contract`).

### Design

- Mode 1 (Claude Project Manual) only.
- Single-brand or multi-brand Project setup.
- Markdown output as default.

---

## Unreleased

Track upcoming improvements here. Suggested directions:

- Additional visual modes.
- More platform-specific carousel sub-frameworks (LinkedIn vs Instagram vs X).
- A/B test prompt for hook variants.
- Automated regression corpus for renderable output.
- Brand Pack diff / merge tooling for refresh cycles.
- More worked examples per industry / niche.
- Additional language support beyond EN/PT/ES coverage in voice examples.
