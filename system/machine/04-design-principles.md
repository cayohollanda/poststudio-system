# Design Principles — PostStudio Machine

> Princípios de design visual aplicados antes de renderizar qualquer slide. Define o que faz um carrossel parecer profissional e não genérico — hierarquia, ritmo, espaçamento, tipografia, componentes, paleta.

---

## 1. HIERARQUIA VISUAL — Regra dos 3 Níveis

Todo slide tem **exatamente 3 níveis de leitura**. Nunca mais, nunca menos.

| Nível | O que é | Peso visual | Exemplo |
|---|---|---|---|
| **1 — Âncora** | O elemento que o olho vê primeiro | Maior: headline condensada, número grande, imagem | `TEMA ABERTO. ÂNGULO SEU.` em 80px |
| **2 — Contexto** | O que explica a âncora | Médio: body text, 38px | Parágrafo explicativo |
| **3 — Metadata** | O que organiza sem competir | Menor: tag, brand bar, progress | `O FENÔMENO` em 13px |

**Regra:** se um slide tem 2 elementos com o mesmo peso visual, um deles precisa mudar. Dois parágrafos do mesmo tamanho = hierarquia quebrada.

### Aplicação prática por tipo de slide

| Tipo | Nível 1 | Nível 2 | Nível 3 |
|---|---|---|---|
| Slide com headline | Headline | Body | Tag |
| Slide sem headline (só body) | `<strong>` no início do body | Resto do texto | Tag |
| Slide com tabela | Header da tabela | Dados | Fonte |
| Slide com Big Stat | Número gigante | Label | Tag |
| Slide com Image Box | Imagem (no topo) | Body abaixo | Tag |
| Capa | Headline | Profile chip | Brand bar |
| CTA | Cta-headline | Keyword box | Frase-ponte + Footer |

---

## 2. RITMO VISUAL — Alternância Dark/Light/Gradient

O template Alternado Claro/Escuro não é só estético — é funcional:

- **Dark slides** = tensão, revelação, mecanismo. Tom mais sério.
- **Light slides** = dados, prova, aplicação prática. Tom mais acessível.
- **Gradient slide** = direção, chamada à ação implícita. Tom de urgência.

### Regra de quebra

**Nunca 3 slides consecutivos do mesmo tipo** (dark-dark-dark ou light-light-light). A alternância mantém o scroll.

### Regra de densidade

Slides dark aguentam menos texto que slides light (fundo escuro cansa mais rápido):

- Max ~80 palavras em dark
- Max ~100 palavras em light
- Gradient: max ~70 palavras (o gradient já cansa visualmente)

---

## 3. ESPAÇAMENTO — Regra do Terço Inferior

O conteúdo textual ocupa o **terço inferior e médio** do slide (`flex-end`). O terço superior fica como respiro visual — não é espaço desperdiçado, é espaço que dá peso ao conteúdo embaixo.

### Exceções onde o topo é preenchido

- Slide com `.img-box` no topo (imagem retangular)
- Slide com `.dark-big-stat` (número gigante no centro-topo)
- Slide de capa (imagem full-bleed)
- Slide com headline interna grande (80px+ preenche naturalmente)

### Se o slide parece "vazio" mesmo com flex-end

Em ordem de tentativa:

1. **Aumentar o font-size** do body ou da headline
2. **Adicionar um card** (`.dark-card` ou `.light-card`) pro texto
3. **Sugerir um `.img-box`** ao usuário (pedir foto)
4. **Último recurso:** adicionar mais conteúdo ao slide (com aprovação do usuário)

Nunca centralizar verticalmente o texto pra preencher visualmente. O slide aceita ar.

---

## 4. TIPOGRAFIA — Escala e Contraste

### Escala fixa (1080×1350px nativos)

| Elemento | Tamanho | Peso | Uso |
|---|---|---|---|
| Headline capa | 88-108px | 900 | Capa do carrossel |
| Headline interna (dark) | 72-80px | 900 | Slides dark com título |
| Headline interna (light) | 64-72px | 900 | Slides light com título |
| Headline grad | 72-80px | 900 | Slide gradient |
| Body | 36-40px | 400 | Texto corrido |
| Body strong | 36-40px | 700-800 | Destaques dentro do body |
| Tag (eyebrow) | 13px | 700 | Labels de seção |
| Brand bar | 13-14px | 600-700 | Topo de cada slide |
| Progress | 15px | 600 | Rodapé |
| Big stat | 200-380px | 900 | Número decorativo |
| Image-box altura | 360px | — | Box retangular de imagem |

### Contraste tipográfico

- **Headline** sempre em fonte condensada (Barlow Condensed / Bebas Neue / Plus Jakarta Sans 800), uppercase, letter-spacing negativo
- **Body** sempre em fonte regular (Plus Jakarta Sans / DM Sans / Inter), sentence case, letter-spacing neutro
- **Nunca usar a mesma fonte/peso/tamanho pra dois elementos diferentes**

### Cor do texto

| Background | Body | Strong | Accent (palavras-chave) |
|---|---|---|---|
| Dark slides | `rgba(255,255,255,0.55)` | `#fff` | `var(--PL)` (primary light) |
| Light slides | `rgba(15,13,12,0.60)` | `var(--DB)` (dark bg, ink) | `var(--P)` (primary) |
| Gradient slides | `rgba(255,255,255,0.65)` | `#fff` | (cor primária NÃO aparece — gradient já tem accent visual) |

**Regra:** accent (cor primária) apenas em **palavras-chave**, nunca em frases inteiras. Máximo 3 palavras em accent por slide.

---

## 5. COMPONENTES VISUAIS — Quando Usar Cada Um

### Card (`.dark-card` / `.light-card`)

- **Usar quando:** texto precisa de destaque extra, citação direta, ou lista de 2-3 itens dentro de um slide
- **Não usar quando:** o slide já tem headline + body (card vira ruído)
- **Características:** border-left de 6-7px na cor primária, padding 44-52px, border-radius 16-18px

### Tabela (`.light-table`)

- **Usar quando:** slide de dados com 3+ itens comparáveis (indicador + valor)
- **Não usar quando:** menos de 3 dados (fica desproporcional)
- **Características:** header em primary com texto branco, rows alternadas, font-family consistente com body

### Big Stat (`.dark-big-stat`)

- **Usar quando:** um único número é o protagonista do slide (ex: "2.300%")
- **Não usar quando:** múltiplos dados de peso igual
- **Características:** 200-380px, weight 900, cor primary, label small embaixo

### Image Box (`.img-box`)

- **Usar quando:** slide com <60% de preenchimento textual E o usuário tem imagem disponível
- **Não usar quando:** slide já está denso ou imagem não adiciona informação
- **Características:** retângulo no topo, altura 360px, border-radius 20px, `object-fit: cover`

### Arrow Rows (`.dark-arrow-row` / `.grad-row`)

- **Usar quando:** slide lista 2-3 pontos sequenciais (não paralelos)
- **Não usar quando:** mais de 4 itens (vira lista, perde impacto)
- **Características:** seta `→` à esquerda, opacity 0.3-0.4, gap 20-22px

### Pattern Card (`.light-pcard`)

- **Usar quando:** slide lista 3-4 padrões com nome + exemplo
- **Não usar quando:** padrões não têm exemplo concreto
- **Características:** card branco com border, número de ordem em primary uppercase pequeno, título em weight 800, exemplo em itálico com border-left

---

## 6. COR — Geração de Paleta a partir da Cor Primária

A partir de uma única cor primária (informada pelo usuário no Briefing), derivar todo o sistema:

```
BRAND_PRIMARY = cor informada pelo usuário (ex: #E8421A)
BRAND_LIGHT  = primary clareado ~20% (mix com branco)            (ex: #FF6B47)
BRAND_DARK   = primary escurecido ~30% (mix com preto)           (ex: #B82F0F)
LIGHT_BG     = off-white com temperatura do primary
                warm primary (vermelho, laranja, amarelo) → #F5F2EF / #F7F4F1
                cool primary (azul, verde, roxo)          → #F0F2F5 / #ECEEF2
                neutral primary (cinza, preto)            → #F4F4F4
DARK_BG      = near-black com leve tint
                warm  → #0F0D0C / #120E0B
                cool  → #0C0D10 / #0B0E14
                neutral → #0A0A0A
LIGHT_BORDER = LIGHT_BG escurecido ~5%                            (ex: #EDE8E3)
GRADIENT     = linear-gradient(165deg, BRAND_DARK 0%, BRAND_PRIMARY 50%, BRAND_LIGHT 100%)
```

### Regras de contraste

- A cor primária **NUNCA aparece como fundo de texto longo**. Sempre como:
  - Accent em palavras-chave (within `<em>` tags)
  - Borda de card (border-left)
  - Fill de progress bar
  - Headlines da capa (palavras-chave)
  - Background de pill/button
- Contraste mínimo texto/fundo: 4.5:1 (WCAG AA)
- Headline em primary sobre fundo light: testar com WebAIM contrast checker

### Fallback se a cor não funcionar

Se o primary tem contraste ruim com o body text (raro, mas possível com amarelos / verdes claros):
- Manter o primary como accent visual (border, progress fill)
- Trocar a cor accent de texto pra primary_dark (escurecido 30%)

---

## 7. IMAGENS — Princípios de Uso

### Capa (obrigatória se imagem disponível)

- Imagem full-bleed com gradiente escuro pesado na base
- Sujeito da foto preferencialmente no terço superior
- Gradiente garante contraste 4.5:1 com headline branca
- Gradiente típico:
  ```css
  background: linear-gradient(
    to bottom,
    rgba(0,0,0,0.35) 0%,
    rgba(0,0,0,0.08) 25%,
    rgba(0,0,0,0.15) 40%,
    rgba(0,0,0,0.65) 55%,
    rgba(0,0,0,0.92) 75%,
    rgba(0,0,0,0.99) 100%
  );
  ```

### Imagem de fundo em slide interno (overlay)

- Apenas em slides dark
- Overlay mínimo 70% opacity
- Texto precisa contraste 4.5:1 mesmo com a imagem mais clara possível
- Usar quando a imagem **adiciona contexto** ao conteúdo (não decoração)

### Image Box (`.img-box`)

- Retângulo com border-radius 20px no topo do conteúdo
- Altura fixa: 360px (ajustar se necessário)
- `object-fit: cover` — nunca distorcer
- Sugerir ao usuário **apenas quando o slide tem espaço sobrando** (<60% preenchido)

### Brand mark do usuário (sempre)

- O brand mark do usuário (logo) NÃO é desenhado por Claude
- Claude **lê o arquivo SVG/PNG anexado** e embeda como base64 (`<image href="data:image/...;base64,...">` ou `<img src="data:...">`)
- **NUNCA redesenhar a marca via SVG primitives, geometria ou aproximação**
- Se o asset não foi anexado: parar e pedir pro usuário anexar

---

## 8. CHECKLIST VISUAL — Rodar Antes de Renderizar

Pra CADA slide do carrossel, verificar:

1. ✅ Hierarquia de 3 níveis clara (âncora, contexto, metadata)
2. ✅ Contraste texto/fundo ≥ 4.5:1
3. ✅ Accent color apenas em palavras-chave, não em frases (max 3 palavras)
4. ✅ Safe area respeitada (56px horizontal, 80px bottom)
5. ✅ Nenhum elemento sobrepõe progress bar ou brand bar
6. ✅ Headline não ultrapassa 4 linhas no tamanho definido
7. ✅ Alternância dark/light respeitada (nunca 3 seguidos do mesmo)
8. ✅ Componente visual correto pro conteúdo (card / tabela / stat / img-box)
9. ✅ Sem swipe arrow (removido — swipe é nativo do Instagram)
10. ✅ CTA tem frase-ponte conectando conteúdo ao call-to-action
11. ✅ Brand mark do usuário embedado (não redesenhado)
12. ✅ Headline da capa cabe em até 5 linhas a 88px (senão derivar versão curta mantendo padrão)

---

## 9. ANTI-PATTERNS VISUAIS — Nunca Fazer

- ❌ Texto centralizado em slides de conteúdo (só CTA pode ter elementos centralizados — keyword box)
- ❌ Dois parágrafos do mesmo tamanho/peso sem diferenciação
- ❌ Imagem sem overlay suficiente comprometendo legibilidade
- ❌ Cor accent em mais de 3 palavras por slide
- ❌ Card dentro de card
- ❌ Tabela com menos de 3 linhas
- ❌ Headline em sentence case (sempre uppercase em condensada)
- ❌ Body text em uppercase (nunca)
- ❌ Mais de 100 palavras em um slide dark
- ❌ Slide com apenas tag + 1 frase curta (parece incompleto — expandir ou fundir com outro)
- ❌ Slide centralizado verticalmente (`justify-content: center`) — sempre `flex-end`
- ❌ Brand mark redesenhado via SVG primitives
- ❌ Mais de 1 emoji por slide (PostStudio Machine evita emojis decorativos)
- ❌ Swipe arrow na borda direita (Instagram já tem swipe nativo)
- ❌ Watermark ou copyright no rodapé (a brand bar já marca autoria)
- ❌ Sombra dramática em texto (text-shadow grosso é AI slop visual)
- ❌ Gradient em background de body text (mata legibilidade)

---

## 10. Adaptação por Estilo Visual

A escolha do estilo no Briefing afeta densidade visual, mas **não** a hierarquia de 3 níveis nem a alternância dark/light:

### Clássico

- Alternado Claro/Escuro padrão
- Tom sóbrio, jornalístico
- Serif nas headlines (Playfair Display 900)
- Body em DM Sans 400
- Accent discreto

### Moderno (default)

- Mais variação visual
- Cards, tabelas, image-boxes usados liberamente
- Sans condensada nas headlines (Barlow Condensed 900)
- Body em Plus Jakarta Sans
- Accent vibrante

### Minimalista

- Maioria light (5 light, 2 dark, 1 gradient, 1 CTA light)
- Mais espaço branco
- Body maior (42px em vez de 38px)
- Headlines mais discretas (64-72px)
- Cards com border-left mais fino (4px)

### Bold

- Maioria dark (5 dark, 2 light, 1 gradient, 1 CTA light)
- Headlines maiores (96-108px nas internas)
- Números decorativos mais visíveis (opacity 0.08 em vez de 0.04)
- Accent saturado
- Mais Big Stats

### Outro

- Interpretar descrição do usuário
- Se vaga, pedir referência visual (ex: "manda screenshot de uma conta que você acha que tem o estilo certo")

---

## 11. O Princípio Final de Design

> **A simplicidade não é estilo — é função.** Um carrossel com 3 níveis claros, alternância respeitada e tipografia consistente já tem 80% da batalha ganha. Os outros 20% são tensão narrativa, dados específicos, e brand mark embedado corretamente.

Anti-pattern visual que mata todos os outros: **cluttered slide** (slide poluído com 4+ elementos competindo por atenção). Sempre que estiver em dúvida, **tire alguma coisa**.
