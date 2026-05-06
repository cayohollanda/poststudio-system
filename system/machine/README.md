# PostStudio Machine — Subsistema

> Núcleo editorial e visual da **PostStudio Machine**: a máquina opinativa de carrosséis do PostStudio. Quando o usuário roda o fluxo guiado completo via [`prompts/20-poststudio-machine.md`](../../prompts/20-poststudio-machine.md), Claude carrega estes 6 arquivos como base de decisão.

---

## Por que existe

O PostStudio System tem 3 modos (Manual / API / Renderable) e ~20 prompts. Pra 80% dos casos, o fluxo padrão (`prompts/04-generate-carousel.md` → `prompts/18-deliver-carousel-as-png-zip.md`) basta.

Os outros 20% são quando o usuário quer:

1. Um **fluxo conversacional guiado** etapa por etapa (briefing → headlines → espinha → texto → imagens → HTML preview → PNG export)
2. **Validação editorial obrigatória** com 7 parâmetros antes do render
3. **10 opções de headlines em tabela** com gatilhos emocionais marcados, com possibilidade de iteração ("ajusta a 3", "mistura 2 com 7")
4. Um **template visual canônico** (Alternado Claro/Escuro) ao invés de escolher entre 9 modos

A PostStudio Machine é esse modo. Esses 6 arquivos são o cérebro dela.

---

## Anatomia

| Arquivo | Função |
|---|---|
| `01-headline-engine.md` | Engine de geração e validação de headlines: padrões de lift, gatilhos emocionais, 3 dimensões de avaliação, banco de hooks de referência, formato de apresentação em tabela |
| `02-editorial-quality.md` | Manual editorial com 7 parâmetros de qualidade (gramática, fluidez, AI slop, fatos, estrutura, densidade, tom). Nota mínima 8/10 em cada. Bloco abaixo de 8 reprova e exige reescrita |
| `03-anti-slop-filter.md` | Lista exaustiva de padrões proibidos: construções binárias, paralelismos forçados, headlines genéricas, vocabulário corporativo, abertura/fechamento fracos. Verificação obrigatória antes do render |
| `04-design-principles.md` | Princípios de design visual: hierarquia de 3 níveis, ritmo dark/light, espaçamento (terço inferior), tipografia (escala fixa), componentes (quando usar card/tabela/stat/img-box), geração de paleta a partir da cor primária |
| `05-template-alternado.md` | Especificação visual completa do template canônico (Alternado Claro/Escuro): variáveis CSS, accent bar, brand bar, progress bar, slide capa, slides dark/light/gradient/CTA, image box, instagram preview frame |
| `06-references.md` | Exemplos de carrosséis completos (triagem → headline → espinha → copy slides) usados como gabarito de qualidade. Padrões extraídos pra auto-validação |
| `07-interview-protocol.md` | **Protocolo Socrático da entrevista.** Define como Claude conduz a entrevista turn-by-turn, regras de detecção de vagueza, 5 etapas (Insumo → Brand → Ângulo → Evidências → Confirmação), web search ativo, anti-patterns de entrevista, cadência ideal |

---

## Como Claude usa

Quando `prompts/20-poststudio-machine.md` é executado, Claude lê os 7 arquivos antes de qualquer interação com o usuário. Em cada etapa do fluxo:

- **Entrevista Socrática (Etapas 1-5 da conversa com o usuário)** consulta `07-interview-protocol.md` pra conduzir as perguntas turn-by-turn, detectar vagueza e fazer fact-check.
- **Pipeline Headlines (após confirmação da entrevista)** consulta `01-headline-engine.md` pra gerar e validar as 10 opções.
- **Validação Editorial (invisível, antes de exibir texto final)** consulta `02-editorial-quality.md` + `03-anti-slop-filter.md` pra reprovar/reescrever blocos abaixo de 8/10.
- **Render HTML (após aprovação de texto + recebimento de imagens)** consulta `04-design-principles.md` + `05-template-alternado.md` pra construir o HTML.
- **Toda a entrevista** consulta `06-references.md` como gabarito de qualidade — cada output deve estar no nível dos exemplos.

---

## Brand-agnostic

Esses 6 arquivos definem **regras universais** do PostStudio. Variáveis específicas da brand (cor, fonte, handle, nicho, CTA) são RUNTIME — coletadas via Briefing Criativo (Bloco 3 do prompt 20) ou herdadas de uma sessão de `prompts/19-ephemeral-brand-intake.md`.

**Nunca** hardcodear brand instances aqui. A `brands/` do framework repo só contém `_template/` — real brands são supridos por sessão.

---

## Crédito de origem

A metodologia editorial e visual destes arquivos foi adaptada de um system prompt de carrosséis de referência publicado por uma máquina de conteúdo de Instagram (BrandsDecoded). Os padrões editoriais (lift de headlines, 7 parâmetros de qualidade, anti-AI-slop) foram validados em milhares de posts. O PostStudio mantém:

- A metodologia de validação editorial
- Os padrões de design (Alternado Claro/Escuro, hierarquia de 3 níveis, escala tipográfica)
- O fluxo conversacional guiado

E adapta:

- Nomenclatura: PostStudio Machine
- Filosofia brand-agnostic: brand é runtime, não commit
- Header chrome: "POWERED BY POSTSTUDIO" / "@poststudio.ai"
- Integração com os outros prompts (00-19) e modos (1/2/3) do PostStudio System

---

## Quando NÃO usar este subsistema

Se o usuário pede só copy de carrossel (sem render) ou só PNG zip de copy já existente, **NÃO** precisa carregar este subsistema. Os prompts 04 e 18 já bastam.

Carregue este subsistema apenas quando o usuário roda explicitamente o prompt 20 (PostStudio Machine — fluxo guiado completo).
