# Template Alternado Claro/Escuro — PostStudio Machine

> Especificação visual completa do template canônico da PostStudio Machine. Todos os slides são **1080×1350px nativos**. Renderização via HTML/CSS + Playwright headless.
>
> **REGRA CRÍTICA DE FONTES:** Nunca usar `<link>` do Google Fonts. Sempre embutir as fontes como base64 via `@font-face` no `<style>` do HTML. Usar pacotes npm `@fontsource/[fonte]` para obter os `.woff2` e converter com `base64 -w0`. Isso garante que o export PNG (Playwright headless) renderize idêntico ao preview no browser.

---

## VARIÁVEIS GLOBAIS (derivadas do briefing)

```css
:root {
  --P:   [BRAND_PRIMARY];          /* ex: #E8421A */
  --PL:  [BRAND_LIGHT];            /* primary clareado ~20% */
  --PD:  [BRAND_DARK];             /* primary escurecido ~30% */
  --LB:  [LIGHT_BG];               /* off-white com temperatura */
  --LR:  [LIGHT_BORDER];           /* LIGHT_BG escurecido ~5% */
  --DB:  [DARK_BG];                /* near-black com tint */
  --G:   linear-gradient(165deg, [PD] 0%, [P] 50%, [PL] 100%);
  --F-HEAD: '[FONTE_HEADLINE]', sans-serif;   /* Barlow Condensed / Playfair / etc */
  --F-BODY: 'Plus Jakarta Sans', sans-serif;
}
```

Variáveis preenchidas conforme tabela de nicho ou cor informada pelo usuário (ver `04-design-principles.md` §6).

---

## ELEMENTOS FIXOS (presentes em TODOS os slides)

### Accent Bar (topo)

```css
.accent-bar {
  position: absolute; top: 0; left: 0; right: 0;
  height: 7px; z-index: 30;
  background: var(--G);
}
.accent-bar.on-grad { background: rgba(255,255,255,0.18); }
```

### Brand Bar

```css
.brand-bar {
  position: absolute; top: 7px; left: 0; right: 0;
  padding: 32px 56px 0;
  display: flex; justify-content: space-between; align-items: center;
  z-index: 20;
  font-family: var(--F-BODY);
  font-size: 13px; font-weight: 700;
  letter-spacing: 1.5px; text-transform: uppercase;
}
.brand-bar.on-light { color: rgba(15,13,12,0.45); }
.brand-bar.on-dark  { color: rgba(255,255,255,0.45); }
.brand-bar.on-grad  { color: rgba(255,255,255,0.50); }
```

**Conteúdo da brand-bar (PostStudio):**

```html
<div class="brand-bar on-[light|dark|grad]">
  <span>POWERED BY POSTSTUDIO</span>
  <span>@[handle_usuario] · [MES YEAR] ®</span>
</div>
```

### Progress Bar

```css
.prog {
  position: absolute; bottom: 0; left: 0; right: 0;
  padding: 0 56px 30px; z-index: 20;
  display: flex; align-items: center; gap: 16px;
  font-family: var(--F-BODY);
}
.prog-track { flex: 1; height: 3px; border-radius: 2px; overflow: hidden; }
.prog-fill  { height: 100%; border-radius: 2px; }
.prog-num   { font-size: 15px; font-weight: 600; }

.on-light .prog-track { background: rgba(0,0,0,0.08); }
.on-light .prog-fill  { background: var(--P); }
.on-light .prog-num   { color: rgba(0,0,0,0.22); }

.on-dark  .prog-track { background: rgba(255,255,255,0.10); }
.on-dark  .prog-fill  { background: #fff; }
.on-dark  .prog-num   { color: rgba(255,255,255,0.22); }

.on-grad  .prog-track { background: rgba(255,255,255,0.15); }
.on-grad  .prog-fill  { background: rgba(255,255,255,0.6); }
.on-grad  .prog-num   { color: rgba(255,255,255,0.30); }
```

Fill calculado proporcionalmente: `((slide_atual / total_slides) * 100)%`.

### Tag / Label (eyebrow)

```css
.tag {
  font-family: var(--F-BODY);
  font-size: 13px; font-weight: 700;
  letter-spacing: 3px; text-transform: uppercase;
  margin-bottom: 24px;
}
.on-light .tag { color: var(--P); }
.on-dark  .tag { color: var(--PL); }
.on-grad  .tag { color: rgba(255,255,255,0.55); }
```

### Área de Conteúdo (padrão)

```css
.content {
  position: absolute;
  top: 110px; left: 56px; right: 56px; bottom: 80px;
  display: flex; flex-direction: column; justify-content: flex-end;
  padding-bottom: 40px;
}
```

**REGRA:** Conteúdo alinhado pela base do slide (`flex-end`), nunca centralizado. Garante que o texto preencha o espaço de baixo pra cima, evitando blocos pequenos flutuando no meio.

---

## SLIDE DE CAPA (Slide 1)

```css
.slide-capa { background: #000; }

.capa-bg {
  position: absolute; inset: 0;
  background: url('[BASE64_IMAGEM_CAPA]') center/cover no-repeat;
}

.capa-grad {
  position: absolute; inset: 0;
  background: linear-gradient(
    to bottom,
    rgba(0,0,0,0.35) 0%,
    rgba(0,0,0,0.08) 25%,
    rgba(0,0,0,0.15) 40%,
    rgba(0,0,0,0.65) 55%,
    rgba(0,0,0,0.92) 75%,
    rgba(0,0,0,0.99) 100%
  );
}

/* Badge do handle — ALINHADO À ESQUERDA, dentro do bloco de capa */
.capa-badge {
  display: flex; align-items: center; gap: 14px;
  background: rgba(0,0,0,0.38);
  border: 1.5px solid rgba(255,255,255,0.12);
  border-radius: 60px;
  padding: 12px 26px 12px 14px;
  backdrop-filter: blur(10px);
  width: fit-content;
  margin-bottom: 32px;
}

.badge-dot {
  width: 36px; height: 36px; border-radius: 50%;
  background: var(--G);
  display: flex; align-items: center; justify-content: center;
  font-family: var(--F-BODY); font-size: 16px; font-weight: 900; color: #fff;
}
.badge-handle {
  font-family: var(--F-BODY);
  font-size: 22px; font-weight: 700; color: #fff; letter-spacing: 0.3px;
}
.badge-check {
  width: 22px; height: 22px; background: var(--P);
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
}

/* Headline da capa — badge + headline ficam num bloco único */
.capa-headline-area {
  position: absolute;
  bottom: 120px; left: 0; right: 0;
  padding: 0 52px;
  z-index: 10;
}
.capa-headline {
  font-family: var(--F-HEAD);
  font-size: 108px; font-weight: 900;
  line-height: 0.93; letter-spacing: -3px; text-transform: uppercase;
  color: #fff;
}
.capa-headline em {
  color: var(--P); font-style: normal;
}
```

**REGRAS DA CAPA:**
- Headline = versão COMPLETA escolhida pelo usuário se cabe em até 5 linhas a 88-108px. Senão, derivar versão curta MANTENDO o padrão (se tinha dois-pontos, manter; se era pergunta, manter).
- Font-size MÍNIMO 88px — nunca abaixo
- Badge fica ACIMA da headline, dentro do `.capa-headline-area`
- SEM badge de tipo/data na capa — capa é limpa: só foto + headline + handle

### Badge de tipo (opcional, NÃO na capa — pode ser usado em slides internos se necessário)

```html
<div class="capa-type-badge">
  <span class="capa-type-label">[TENDÊNCIA | ANÁLISE | CASE | PREVISÃO]</span>
  <span class="capa-date">[DD/MM/AAAA]</span>
</div>
```

Tipo derivado do briefing:
- Tendência Interpretada → `TENDÊNCIA`
- Tese Contraintuitiva → `ANÁLISE`
- Case/Benchmark → `CASE`
- Previsão/Futuro → `PREVISÃO`

---

## SLIDE INTERNO — DARK (Slides 2, 4, 6)

```css
.slide-dark { background: var(--DB); }

.slide-dark .content { justify-content: flex-end; padding-bottom: 40px; }

/* Headline principal */
.dark-h1 {
  font-family: var(--F-HEAD);
  font-size: 80px; font-weight: 900;
  line-height: 0.97; letter-spacing: -2px; text-transform: uppercase;
  color: #fff; margin-bottom: 36px;
}
.dark-h1 em { color: var(--P); font-style: normal; }

/* Número grande decorativo (background) */
.dark-bg-num {
  position: absolute; right: -10px; bottom: 50px;
  font-family: var(--F-HEAD);
  font-size: 380px; font-weight: 900;
  color: rgba(255,255,255,0.04);
  line-height: 1; letter-spacing: -14px;
  pointer-events: none; z-index: 1;
}

/* Body text */
.dark-body {
  font-family: var(--F-BODY);
  font-size: 38px; font-weight: 400;
  line-height: 1.5; letter-spacing: -0.2px;
  color: rgba(255,255,255,0.55);
}
.dark-body strong { color: #fff; font-weight: 700; }
.dark-body em     { color: var(--PL); font-style: normal; }

/* Arrow items */
.dark-arrow-row {
  display: flex; align-items: flex-start; gap: 20px; margin-bottom: 28px;
}
.dark-arrow-row::before {
  content: '→'; font-size: 32px; color: rgba(255,255,255,0.3);
  flex-shrink: 0; margin-top: 4px;
  font-family: var(--F-BODY);
}

/* Highlight card */
.dark-card {
  background: rgba(255,255,255,0.04);
  border-left: 6px solid var(--P);
  border-radius: 16px; padding: 44px 48px;
}

/* Big number stat */
.dark-big-stat {
  font-family: var(--F-HEAD);
  font-size: 200px; font-weight: 900;
  line-height: 1; letter-spacing: -8px;
  color: var(--P); margin-bottom: -10px;
}
.dark-stat-label {
  font-family: var(--F-BODY);
  font-size: 30px; font-weight: 500;
  color: rgba(255,255,255,0.35); margin-bottom: 48px;
}
```

---

## SLIDE INTERNO — LIGHT (Slides 3, 5, 7)

```css
.slide-light { background: var(--LB); }

.slide-light .content { justify-content: flex-end; padding-bottom: 40px; }

/* Headline */
.light-h1 {
  font-family: var(--F-HEAD);
  font-size: 72px; font-weight: 900;
  line-height: 1.0; letter-spacing: -1.5px; text-transform: uppercase;
  color: var(--DB); margin-bottom: 32px;
}
.light-h1 em { color: var(--P); font-style: normal; }

/* Body text */
.light-body {
  font-family: var(--F-BODY);
  font-size: 38px; font-weight: 400;
  line-height: 1.55; letter-spacing: -0.2px;
  color: rgba(15,13,12,0.60);
}
.light-body strong { color: var(--DB); font-weight: 800; }

/* Card branco */
.light-card {
  background: #fff;
  border-left: 7px solid var(--P);
  border-radius: 18px; padding: 52px 56px;
}

/* Pattern card (para padrões/listas) */
.light-pcard {
  background: #fff;
  border-radius: 18px; padding: 40px 48px;
  border: 1.5px solid var(--LR);
  margin-bottom: 20px;
}
.light-pnum {
  font-size: 11px; font-weight: 700;
  letter-spacing: 3px; text-transform: uppercase;
  color: var(--P); margin-bottom: 14px;
}
.light-ptitle {
  font-family: var(--F-BODY);
  font-size: 30px; font-weight: 800;
  color: var(--DB); margin-bottom: 16px; letter-spacing: -0.3px;
}
.light-pex {
  font-family: var(--F-BODY);
  font-size: 24px; font-weight: 400; line-height: 1.45;
  color: rgba(15,13,12,0.50); font-style: italic;
  border-left: 4px solid var(--P); padding-left: 22px;
}

/* Data table */
.light-table { width: 100%; border-collapse: collapse; }
.light-table th {
  background: var(--P); color: #fff;
  padding: 20px 24px; font-size: 16px; font-weight: 700;
  letter-spacing: 2px; text-transform: uppercase; text-align: left;
  font-family: var(--F-BODY);
}
.light-table td {
  padding: 22px 24px; font-size: 26px; font-weight: 500;
  border-bottom: 1px solid var(--LR);
  font-family: var(--F-BODY);
}
.light-table tr:last-child td { border-bottom: none; }
```

---

## SLIDE INTERNO — GRADIENT (Slide 8 / penúltimo)

```css
.slide-grad { background: var(--G); }

.slide-grad .content { justify-content: flex-end; padding-bottom: 40px; }

/* Número decorativo de fundo */
.grad-bg-num {
  position: absolute; right: -15px; bottom: 40px;
  font-family: var(--F-HEAD);
  font-size: 420px; font-weight: 900;
  color: rgba(255,255,255,0.06);
  line-height: 1; letter-spacing: -16px;
  pointer-events: none;
}

/* Headline */
.grad-h1 {
  font-family: var(--F-HEAD);
  font-size: 80px; font-weight: 900;
  line-height: 0.97; letter-spacing: -2px; text-transform: uppercase;
  color: #fff; margin-bottom: 40px;
}

/* Body */
.grad-body {
  font-family: var(--F-BODY);
  font-size: 38px; font-weight: 400;
  line-height: 1.55; letter-spacing: -0.2px;
  color: rgba(255,255,255,0.65);
}
.grad-body strong { color: #fff; font-weight: 700; }

/* Arrow rows */
.grad-row {
  display: flex; align-items: flex-start; gap: 22px; margin-bottom: 30px;
}
.grad-arrow {
  font-size: 34px; color: rgba(255,255,255,0.4);
  flex-shrink: 0; margin-top: 4px;
  font-family: var(--F-BODY);
}
.grad-text {
  font-family: var(--F-BODY);
  font-size: 32px; font-weight: 500;
  line-height: 1.45; letter-spacing: -0.2px;
  color: rgba(255,255,255,0.72);
}
.grad-text strong { color: #fff; font-weight: 800; }
```

**REGRA do título do gradient:** uma frase de IMPACTO curta (2-4 palavras), não um conselho. Pode ser provocação, dado ou nome. Exemplos: "Identidade ou extinção." / "3 sinais. 1 ano." / "200 clubes em SP." — nunca "Tema aberto. Posição sua." (genérico).

---

## SLIDE DARK COM IMAGEM INTERNA (Slides 4, 6 quando há imagens extras)

Quando o usuário envia mais de 1 imagem, slides dark recebem imagem de fundo com overlay pesado.

```css
.slide-img-bg {
  position: absolute; inset: 0;
  background-size: cover;
  background-position: center;
  z-index: 0;
}

.slide-img-overlay {
  position: absolute; inset: 0;
  background: linear-gradient(
    to bottom,
    rgba(4,4,22,0.80) 0%,
    rgba(4,4,22,0.70) 30%,
    rgba(4,4,22,0.75) 60%,
    rgba(4,4,22,0.90) 100%
  );
  z-index: 1;
}

.slide-dark.with-img .content { z-index: 2; }
```

**REGRAS:**
- Overlay NUNCA menor que 70% opacity. Legibilidade prevalece sobre a imagem.
- TODAS as imagens enviadas pelo usuário devem ser usadas. Nunca ignorar.

---

## IMAGE BOX (para slides com espaço sobrando)

Quando um slide tem <60% de preenchimento textual, sugerir image-box no topo. Disponível em slides light e dark.

```css
.img-box {
  width: 100%;
  height: 360px;
  border-radius: 20px;
  overflow: hidden;
  margin-bottom: 36px;
}
.img-box img {
  width: 100%; height: 100%;
  object-fit: cover;
}

.on-dark .img-box  { border: 1.5px solid rgba(255,255,255,0.08); }
.on-light .img-box { box-shadow: 0 4px 24px rgba(0,0,0,0.06); }
```

**REGRA:** image-box é SUGERIDO, nunca imposto. O usuário decide.

---

## SLIDE CTA (último slide)

Layout assimétrico, alinhado à esquerda. Não centralizado (exceto conteúdo dentro do `.cta-kbox` que pode ser centralizado).

```css
.slide-cta { background: var(--LB); }

.slide-cta .content {
  justify-content: flex-end;
  padding-bottom: 40px;
}

/* Frase-ponte — conecta o conteúdo ao CTA */
.cta-bridge {
  font-family: var(--F-BODY);
  font-size: 38px; font-weight: 500;
  line-height: 1.5; letter-spacing: -0.2px;
  color: rgba(15,13,12,0.55);
  margin-bottom: 48px;
}
.cta-bridge strong { color: var(--DB); font-weight: 800; }

/* Headline do CTA */
.cta-headline {
  font-family: var(--F-HEAD);
  font-size: 72px; font-weight: 900;
  line-height: 0.97; letter-spacing: -2px; text-transform: uppercase;
  color: var(--DB); margin-bottom: 40px;
}
.cta-headline em { color: var(--P); font-style: normal; }

/* Keyword box — palavra-chave do CTA */
.cta-kbox {
  background: #fff;
  border: 3px solid rgba(232,66,26,0.15);  /* ~15% do primary */
  border-radius: 20px; padding: 40px 48px;
  margin-bottom: 32px;
}
.cta-kinstr {
  font-family: var(--F-BODY);
  font-size: 20px; font-weight: 500;
  color: rgba(15,13,12,0.42); margin-bottom: 12px;
}
.cta-kword {
  font-family: var(--F-HEAD);
  font-size: 80px; font-weight: 900;
  color: var(--P); letter-spacing: -2px; line-height: 1; margin-bottom: 14px;
}
.cta-kbenefit {
  font-family: var(--F-BODY);
  font-size: 22px; font-weight: 500; line-height: 1.5;
  color: rgba(15,13,12,0.50);
}

/* Footer do CTA */
.cta-footer {
  display: flex; align-items: center; gap: 16px;
}
.cta-footer-dot {
  width: 40px; height: 40px; border-radius: 50%;
  background: var(--G);
  display: flex; align-items: center; justify-content: center;
  font-family: var(--F-BODY); font-size: 16px; font-weight: 900; color: #fff;
}
.cta-footer-text {
  font-family: var(--F-BODY);
  font-size: 18px; color: rgba(15,13,12,0.35);
}
```

**HTML do CTA:**

```html
<div class="slide slide-cta on-light" id="slide-9">
  <div class="accent-bar"></div>
  <div class="brand-bar on-light">
    <span>POWERED BY POSTSTUDIO</span>
    <span>@[handle] · [MES YEAR] ®</span>
  </div>
  <div class="content">
    <div class="cta-bridge">
      [Frase-ponte: conecta o último insight do carrossel ao CTA — ex: "Os dados mostram que distribuição vem antes de qualidade. Quem ignora isso produz pra <strong>própria bolha</strong>."]
    </div>
    <div class="cta-headline">QUER O<br><em>MANUAL</em><br>COMPLETO?</div>
    <div class="cta-kbox">
      <div class="cta-kinstr">Comenta a palavra abaixo:</div>
      <div class="cta-kword">MANUAL</div>
      <div class="cta-kbenefit">e recebe o guia direto na DM</div>
    </div>
    <div class="cta-footer">
      <div class="cta-footer-dot">[INICIAL]</div>
      <span class="cta-footer-text">@[handle_usuario] · Envio automático via DM</span>
    </div>
  </div>
  <div class="prog">
    <div class="prog-track"><div class="prog-fill" style="width: 100%;"></div></div>
    <div class="prog-num">9/9</div>
  </div>
</div>
```

**REGRA CTA OBRIGATÓRIA:**
- Frase-ponte é OBRIGATÓRIA. Conecta o último insight do carrossel ao CTA. Nunca um CTA genérico desconectado do conteúdo.
- Layout alinhado à esquerda, nunca centralizado (exceto dentro do `.cta-kbox`).

---

## SEQUÊNCIA — Alternado Claro/Escuro

### 9 slides (padrão)

```
Slide 1: Capa (foto full-bleed + headline)
Slide 2: Dark (hook)
Slide 3: Light (contexto / mecanismo pt.1)
Slide 4: Dark (mecanismo pt.2)
Slide 5: Light (prova / dados)
Slide 6: Dark (expansão)
Slide 7: Light (aplicação)
Slide 8: Grad (direção)
Slide 9: Light CTA
```

### 7 slides

```
1: Capa | 2: Hook (Dark) | 3: Mecanismo (Light) | 4: Prova (Dark) | 5: Expansão (Light) | 6: Direção (Grad) | 7: CTA (Light)
```

### 5 slides

```
1: Capa | 2: Hook+Contexto (Dark) | 3: Prova (Light) | 4: Aplicação+Direção (Dark) | 5: CTA (Light)
```

### 12 slides

```
1: Capa | 2: Hook (Dark) | 3: Contexto (Light) | 4: Mecanismo pt.1 (Dark) | 5: Mecanismo pt.2 (Light) |
6: Prova principal (Dark) | 7: Dados secundários (Light) | 8: Expansão (Dark) | 9: Caso prático (Light) |
10: Aplicação (Dark) | 11: Direção (Grad) | 12: CTA (Light)
```

**Regra invariável:** os últimos 3 slides sempre fazem a transição narrativa pro CTA. Nunca o CTA é o único slide de fechamento sem preparação.

---

## GARANTIAS DE LEGIBILIDADE (verificar antes de renderizar)

1. **Contraste texto/fundo:** mínimo 4.5:1 — se imagem de fundo clara, escurecer gradiente
2. **Headline na capa:** nunca ultrapassa 4 linhas; se sim, reduzir font-size em 12px e testar
3. **Body text:** nunca sobrepõe progress bar — padding-bottom mínimo 80px em todos os slides
4. **Palavras em cor accent:** apenas palavras-chave, nunca linhas inteiras (max 3 palavras/slide)
5. **Brand bar:** sempre legível — opacidade 45-50%, font-size 13-14px
6. **Safe area horizontal:** mínimo 56px de cada lado para todo texto
7. **Sem swipe arrow** — swipe é nativo do Instagram, nenhum elemento visual de seta nos slides

---

## PREVIEW LADO A LADO (no HTML)

O HTML deve incluir um modo preview onde todos os slides aparecem lado a lado em miniatura (30% scale). Permite avaliar o ritmo visual do carrossel inteiro.

```css
.slides-wrap {
  display: flex;
  flex-wrap: wrap;
  gap: 18px;
  justify-content: center;
  max-width: 1400px;
  margin: 0 auto;
}

.slide.preview-mode {
  transform: scale(0.30);
  transform-origin: top left;
  margin-bottom: -945px;  /* compensa altura reduzida */
  margin-right: -756px;   /* compensa largura reduzida */
}
```

**Toggle:** botão no topo do HTML alterna Preview ↔ Full Size.

```html
<button onclick="toggleView()" style="position:fixed;top:16px;right:16px;z-index:1000;padding:12px 20px;background:#000;color:#fff;border:none;border-radius:8px;cursor:pointer;font-family:system-ui;font-size:14px;font-weight:600;">
  Alternar Preview / Tamanho Real
</button>
<script>
function toggleView() {
  document.querySelectorAll('.slide').forEach(s => s.classList.toggle('preview-mode'));
}
</script>
```

---

## INSTAGRAM FRAME PREVIEW (opcional)

Antes dos slides em tamanho real, renderizar mockup de como o carrossel aparecerá no feed:

```css
.ig-frame {
  width: 420px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 20px rgba(0,0,0,0.1);
  overflow: hidden;
  margin: 0 auto 40px;
}

.ig-header {
  display: flex; align-items: center; gap: 12px;
  padding: 14px 16px;
}
.ig-avatar {
  width: 36px; height: 36px; border-radius: 50%;
  background: var(--G);
  display: flex; align-items: center; justify-content: center;
  font-size: 14px; font-weight: 900; color: #fff;
}
.ig-handle    { font-size: 14px; font-weight: 600; color: #262626; }
.ig-handle-sub{ font-size: 12px; color: #8e8e8e; }

.ig-viewport {
  width: 420px; height: 525px;
  overflow: hidden; position: relative;
}

.ig-dots {
  display: flex; justify-content: center; gap: 4px;
  padding: 12px 0;
}
.ig-dot      { width: 6px; height: 6px; border-radius: 50%; background: #c4c4c4; }
.ig-dot.active { background: #0095f6; }

.ig-actions { display: flex; gap: 16px; padding: 8px 16px; }
.ig-caption { padding: 4px 16px 16px; font-size: 13px; color: #262626; line-height: 1.4; }
```

**REGRA:** Instagram Frame é PREVIEW — não é exportado como PNG. Serve apenas pra visualizar como vai ficar no feed. Os PNGs exportados são os slides 1080×1350 puros.

---

## EXEMPLO HTML COMPLETO (slide dark com card)

```html
<div class="slide slide-dark on-dark" id="slide-2">
  <div class="accent-bar"></div>
  <div class="brand-bar on-dark">
    <span>POWERED BY POSTSTUDIO</span>
    <span>@weaura.tech · MAY 2026 ®</span>
  </div>
  <div class="dark-bg-num">02</div>
  <div class="content">
    <div class="tag">O FENÔMENO</div>
    <h1 class="dark-h1">Em 2020 todo mundo foi correr. As academias abriram <em>e ninguém parou</em>.</h1>
    <p class="dark-body">
      <strong>34% mais brasileiros</strong> correm hoje do que há dois anos. Não é tendência fitness — é fenômeno cultural que precisa de explicação.
    </p>
  </div>
  <div class="prog">
    <div class="prog-track"><div class="prog-fill" style="width: 22%;"></div></div>
    <div class="prog-num">2/9</div>
  </div>
</div>
```

---

## REGRAS DE EMBED DE FONTES

```html
<style>
  @font-face {
    font-family: 'Barlow Condensed';
    font-style: normal;
    font-weight: 900;
    font-display: block;
    src: url(data:font/woff2;base64,[BASE64_DA_FONTE]) format('woff2');
  }
  @font-face {
    font-family: 'Plus Jakarta Sans';
    font-style: normal;
    font-weight: 400;
    font-display: block;
    src: url(data:font/woff2;base64,[BASE64_DA_FONTE]) format('woff2');
  }
  /* ... outras variações de peso ... */
</style>
```

**Como obter o base64 das fontes (no sandbox de execução):**

```python
import base64, urllib.request

# Opção A: instalar via npm @fontsource/* e ler do disco
# (No Claude.ai sandbox, fonts costumam estar pré-instaladas em /usr/share/fonts/)

# Opção B: baixar direto do CDN do Google Fonts
url = "https://fonts.gstatic.com/s/barlowcondensed/v12/HTxwL3I-JCGChYJ8VI-L6OO_au7B7Q.woff2"
with urllib.request.urlopen(url) as r:
    woff2 = r.read()
b64 = base64.b64encode(woff2).decode('ascii')
print(b64)
```

`font-display: block` faz o renderer aguardar a fonte carregar antes de exibir o texto (essencial pro Playwright).

---

## SCRIPT DE EXPORT VIA PLAYWRIGHT

```python
from playwright.sync_api import sync_playwright
import os, zipfile

HTML_PATH = "/tmp/poststudio-machine/carousel.html"
OUT_DIR = "/tmp/poststudio-machine/slides"
ZIP_PATH = "/tmp/poststudio-machine/carousel.zip"
os.makedirs(OUT_DIR, exist_ok=True)

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page(viewport={"width": 1200, "height": 1400})
    page.goto(f"file://{os.path.abspath(HTML_PATH)}", wait_until="networkidle")

    page.wait_for_timeout(2000)
    page.evaluate("() => document.fonts.ready")
    page.wait_for_timeout(2000)

    # Garantir que NÃO está em modo preview
    page.evaluate("""() => {
        document.querySelectorAll('.slide').forEach(s => s.classList.remove('preview-mode'));
    }""")

    slides = page.locator(".slide")
    for i in range(slides.count()):
        slide = slides.nth(i)
        slide.scroll_into_view_if_needed()
        page.wait_for_timeout(300)
        slide.screenshot(path=f"{OUT_DIR}/slide_{i+1:02d}.png")

    browser.close()

with zipfile.ZipFile(ZIP_PATH, "w") as z:
    for f in sorted(os.listdir(OUT_DIR)):
        z.write(os.path.join(OUT_DIR, f), arcname=f)
```

**Regras absolutas:**
- `slide.screenshot()` no ELEMENTO `.slide` — NUNCA `page.screenshot()` no viewport. Garante 1080×1350 exatos sem clip/scale/resize.
- Sempre `document.fonts.ready` antes de capturar — nunca confiar só em timeout.
- Se a fonte não carregar mesmo após espera, o HTML deve ter fallback chain visualmente aceitável.
- Salvar PNGs e zip em path acessível ao usuário.
- Anexar zip + PNGs individuais no chat.

---

## CHECKLIST FINAL ANTES DE ENTREGAR HTML

- [ ] Variáveis CSS preenchidas com paleta correta
- [ ] Fontes embedadas via `@font-face` base64 (não `<link>`)
- [ ] Brand bar com `POWERED BY POSTSTUDIO` + `@[handle] · [MES YEAR] ®`
- [ ] Capa: imagem full-bleed + gradiente + badge handle + headline em até 5 linhas
- [ ] Slides internos: alternância dark/light/grad respeitada (nunca 3 seguidos do mesmo)
- [ ] Brand mark do usuário embedado (não redesenhado)
- [ ] Progress bar com fill proporcional + número N/M
- [ ] Sem swipe arrow
- [ ] CTA com frase-ponte conectando ao conteúdo
- [ ] Modo preview lado-a-lado + modo full size + toggle
- [ ] (Opcional) Instagram Frame Preview no topo

Quando tudo OK → entregar HTML, aguardar comando "exportar" do usuário pra rodar Playwright.
