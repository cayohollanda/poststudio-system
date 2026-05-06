# Anti-AI-Slop Filter — PostStudio Machine

> Lista exaustiva de padrões textuais proibidos. Verificar TODOS os blocos de copy antes de renderizar qualquer slide. Se qualquer padrão abaixo aparecer: reescrever, nunca manter.

---

## 1. PADRÕES PROIBIDOS — Frases e Construções

### 1.1 Construções diretas proibidas

| Proibido | Por quê | Alternativa |
|---|---|---|
| "Não é X, é Y" | Construção desgastada, soa formulaico | Mostrar a diferença sem nomear a fórmula |
| "E isso muda tudo" | Hipérbole vazia, não prova nada | Dizer especificamente o que muda e como |
| "No fim das contas" | Transição de fechamento genérica | Cortar ou reescrever sem transição |
| "Ao final do dia" | Anglicismo desgastado | Cortar |
| "A pergunta fica:" | Encerramento fraco, não cria tensão | Formular a pergunta sem anunciá-la |
| "De forma X" | Vaga, preenche espaço sem informar | Ser específico sobre como |
| "Claro/clara" genérico | "É claro que X" — padding | Cortar o "é claro que" |
| "Cada vez mais" | Lugar-comum sem dado | Usar o dado real em vez da sensação |
| "Em um mundo onde" | Abertura de redação escolar | Começar direto no fato |
| "Vivemos em uma era" | Idem | Idem |
| "É preciso" / "Devemos" | Tom de coach, 2ª pessoa implícita | Tom jornalístico — descrever, não prescrever |
| "Você precisa" / "Você deve" | 2ª pessoa — proibido em slides | Reescrever como reportagem |
| "Simplesmente" | Minimiza sem informar | Cortar |
| "Basicamente" | Hedge desnecessário | Cortar |
| "Na prática" (como abertura) | Transição preguiçosa | Ir direto pra prática |
| "Vale lembrar" | Transição preguiçosa | Cortar |
| "Vale ressaltar" | Idem | Idem |
| "É importante notar" | Idem + meta-comentário | Cortar |
| "Sem dúvidas" / "Com certeza" | Hedge contraproducente | Cortar |
| "Interessante notar" | Meta-comentário | Cortar |

### 1.2 Paralelismos forçados (proibidos)

```
❌ "X diminui, Y acelera"
❌ "Enquanto X perde, Y ganha"
❌ "Menos X, mais Y"
❌ "Antes: X. Agora: Y."
❌ "Sem X. Sem Y."
❌ "Mais X. Menos Y."
❌ "Onde X, Y."
```

Quando o contraste for real e necessário, escrever em prosa com conector natural — não em fórmula de dois pontos.

### 1.3 Headlines e hooks proibidos

```
❌ "Quando X vira Y"
❌ "A ascensão de X"
❌ "O impacto de X"
❌ "Por que X está mudando Y" (genérico, sem reenquadramento)
❌ "X: o que você precisa saber"
❌ "Tudo que você precisa saber sobre X"
❌ "O guia definitivo de X"
❌ "X mudou para sempre"
❌ "Virou" como verbo principal de headline ("o mercado virou")
❌ "Revelamos" / "Descobrimos" / "Mostramos"
❌ Qualquer headline com "descubra", "saiba", "conheça" como abertura
❌ "5 dicas para...", "10 erros que...", "7 segredos de..."
❌ "Você não vai acreditar"
❌ "Isso vai mudar como você pensa sobre..."
❌ "O que ninguém te conta sobre..."
```

---

## 2. TESTE DO TOM DE IA

Leia o bloco em voz alta. Se qualquer uma destas perguntas tiver resposta "sim", reescreva:

1. Qualquer conta do Instagram com mais de 10k seguidores poderia ter escrito isso?
2. O bloco poderia funcionar para qualquer nicho sem trocar uma palavra?
3. Tem frase que soa como conclusão de redação do ENEM?
4. Tem alguma palavra que nenhum jornalista do Estadão usaria?
5. O bloco motiva sem informar nada concreto?

Se sim pra qualquer uma → reescrever com dado específico, ângulo concreto ou tensão real.

---

## 3. PADRÕES PROIBIDOS — Estrutura

### 3.1 Abertura de slides proibida

```
❌ "Hoje vamos falar sobre..."
❌ "Neste carrossel você vai aprender..."
❌ "Antes de começar..."
❌ "Como você provavelmente sabe..."
❌ "Muitas pessoas perguntam..."
❌ "Todo mundo já ouviu falar de..."
❌ "Existe uma teoria que diz..."
❌ "Vou te explicar..."
❌ "Repare bem em..."
```

Slides começam no fato, na tensão ou no dado — nunca na preparação ou meta-comentário.

### 3.2 Fechamento de slides proibido

```
❌ "Continue no próximo slide →"
❌ "Swipe para ver mais"
❌ "Mas tem mais..."
❌ "Não para por aí"
❌ "E o melhor está por vir"
❌ "Veja na próxima imagem"
❌ "Continua..."
```

O próximo slide deve ser inevitável pela tensão narrativa, não pelo aviso explícito.

### 3.3 Fechamento de CTA proibido

```
❌ "Espero que tenha gostado!"
❌ "Se tiver dúvidas, me manda mensagem"
❌ "Obrigado por acompanhar"
❌ "Não esqueça de seguir"
❌ "Se esse conteúdo te ajudou..."
❌ "Te vejo no próximo!"
❌ "Compartilha com aquele amigo que precisa ver isso"
❌ "Salva pra não esquecer"
```

CTA é diretivo, não cordial. "Comenta X, recebe Y" — sem agradecimento, sem despedida.

---

## 4. PADRÕES PROIBIDOS — Dados e Fontes

```
❌ "Estudos mostram que..."     → sempre nomear o estudo + ano
❌ "Especialistas dizem..."     → sempre nomear quem
❌ "Muitas empresas..."         → sempre dar número ou exemplo
❌ "A maioria das pessoas..."   → sempre dar percentual + fonte
❌ "Recentemente..."            → sempre dar data ou período
❌ "No Brasil..."               → sempre dar dado específico do Brasil
❌ "Pesquisa da [vaga]..."      → nome completo + ano da pesquisa
❌ "Análises mostram..."        → quem analisou? quando? quantos casos?
```

**Regra:** número + fonte + ano. Sem os três, não é dado — é opinião.

Se Claude não tem como verificar (sem ferramenta de pesquisa), marcar `[PROOF_NEEDED: descrição curta]` e surfacear em Missing Inputs.

---

## 5. PADRÕES PROIBIDOS — Vocabulário

### 5.1 Jargões a evitar (quando existe equivalente coloquial)

| Jargão | Substituir por |
|---|---|
| "Ecossistema" (genérico) | sistema, mercado, ambiente |
| "Sinergia" | integração, colaboração |
| "Disruptivo" | que quebra com o padrão, que muda o jogo |
| "Stakeholders" | envolvidos, partes interessadas |
| "Mindset" | mentalidade, modo de pensar |
| "Engajamento" (vago) | resultado, performance, alcance |
| "Curadoria" (genérico) | seleção, escolha criteriosa |
| "Storytelling" | narrativa, história |
| "Overview" | visão geral, panorama |
| "Benchmark" (verbo) | comparar com referências |
| "Touchpoint" | ponto de contato |
| "Funil" (sem contexto) | jornada de compra |
| "Pipeline" (genérico) | sequência, fluxo |
| "Deliverable" | entrega, resultado |
| "Insights" (vago) | leitura, observação, conclusão |
| "Empoderar" | dar capacidade, dar autonomia |
| "Escalável" (genérico) | que cresce sem custo proporcional |
| "Otimizar" | melhorar, ajustar |
| "Engajar" (vago) | conquistar atenção, conectar com |

**Exceção:** se o jargão é parte da identidade do nicho (ex: "CAC", "MRR", "CTR", "K8s", "RAG") — pode manter, mas explicar na primeira ocorrência se o público alvo for misto.

### 5.2 Anglicismos numéricos — proibidos em texto corrido

❌ "10+ anos" → ✅ "mais de 10 anos"
❌ "5x maior" → ✅ "cinco vezes maior"
❌ "2-3 anos" → ✅ "dois ou três anos"
❌ "$10MM" → ✅ "10 milhões de dólares" ou "R$ 50 milhões"
❌ "Q3" sozinho → ✅ "terceiro trimestre"

### 5.3 Construções genéricas — proibidas

❌ "de forma clara"
❌ "de forma consistente"
❌ "de forma natural"
❌ "de forma orgânica"
❌ "de maneira eficiente"
❌ "de algum modo"

---

## 6. REGRAS GRAMATICAIS OBRIGATÓRIAS

1. **Artigos sempre presentes** — nunca omitir um/uma/o/a pra economizar espaço
2. **Conectivos naturais em cada bloco** — porque, só que, por isso, enquanto, quando, mas, aí, então, daí
3. **Cada bloco soa como parágrafo de reportagem** — não como lista disfarçada de frase
4. **Nunca cortar conectivo pra caber no espaço** — reescrever a ideia completa menor

---

## 7. CHECKLIST RÁPIDO (antes de entregar)

Rodar em cada bloco de copy:

- [ ] Tem dado específico com fonte quando faz afirmação factual?
- [ ] Não usa nenhuma construção da lista 1.1?
- [ ] Não usa paralelismo forçado da lista 1.2?
- [ ] Não usa headline proibida da lista 1.3?
- [ ] Passa no teste do tom de IA (seção 2)?
- [ ] Cada bloco tem conectivo natural?
- [ ] Artigos presentes em todas as frases?
- [ ] Abertura do slide vai direto ao fato/tensão/dado (não usa lista 3.1)?
- [ ] Fechamento do slide abre espaço pro próximo sem anunciar (não usa lista 3.2)?
- [ ] CTA é diretivo sem agradecimento (não usa lista 3.3)?
- [ ] Sem jargão da lista 5.1?
- [ ] Sem anglicismo numérico da lista 5.2?
- [ ] Sem construção genérica da lista 5.3?

Se qualquer item falhar, reescrever o bloco antes de avançar pra Etapa 3.7 (Aprovação de Texto).

---

## 8. O Princípio Editorial Final

A diferença entre conteúdo bom e AI slop não é o assunto, nem a profundidade técnica. É o **ritmo, o vocabulário e a tensão**. Um bloco bem escrito pode ser sobre Kubernetes, sobre hábitos da Gen Z ou sobre nutrição — e ainda assim soar como reportagem de qualidade, não como saída de modelo.

Quando estiver em dúvida: **leia em voz alta**. Se travou, soou apresentação ou pareceu lista disfarçada, refaça.
