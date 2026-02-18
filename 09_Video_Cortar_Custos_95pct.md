# Vídeo: "I Cut My OpenClaw Costs by 95% (Here's How)"

**Canal:** Nick Puru | AI Automation (66K subscribers)
**URL:** https://youtu.be/o0n2IBByGJM
**Duração:** 18 minutos 6 segundos
**Data:** Feb 7, 2026 | 11K views

---

## Resumo Executivo

Nick Puru estava a gastar **$50/semana** na API do Anthropic sem saber. Após fazer um audit completo, descobriu que **85% do gasto era desperdício puro**. Aplicando 3 camadas de fixes, baixou para **$0.50-$1/semana** — mesma funcionalidade, muitas vezes mais rápido.

---

## O Problema Raiz

O OpenClaw faz chamadas à API em background **constantemente**, mesmo quando não estás a usar. Inclui:
- Heartbeats a cada 30-60 minutos
- Carregamento de contexto em cada interação
- Tarefas simples tratadas por modelos caros

### Breakdown do Gasto (antes dos fixes)
| Categoria | % do gasto | O quê |
|-----------|-----------|-------|
| Overhead puro | 60% | Heartbeats + context loading não solicitados |
| Spend mal direcionado | 25% | Tarefas simples em modelos premium |
| Trabalho produtivo real | **15%** | O que realmente pediste ao agente |

> "About 85% of my money was either just wasted entirely or poorly allocated."

---

## Camada 1 — Eliminar o Overhead Invisível (resolve os 60%)

### Fix A: Redirecionar Heartbeat para Modelo Local (Ollama)

**O problema:** O heartbeat dispara a cada 30-60 min e carrega o contexto COMPLETO (personalidade, memória, histórico) = dezenas de milhares de tokens por chamada. 24/7, mesmo enquanto dormes.

**A solução:**
1. Instalar **Ollama** (gratuito, corre localmente)
2. Descarregar modelo pequeno: **Llama 3B** (≈2GB, corre em qualquer máquina)
3. Atualizar configuração do OpenClaw para apontar heartbeat ao modelo local
4. Resultado: verificações de status gratuitas, sem contar para rate limits

**Impacto:** ~40% do spend total foi a zero da noite para o dia.

*Nota: Nick tem outro vídeo sobre como instalar o Ollama.*

---

### Fix B: Controlar o que Carrega no Contexto

**O problema:** Em cada mensagem, o agente carrega TUDO — histórico de semanas, todos os ficheiros do workspace. Uma pergunta simples ("O que tenho no calendário?") chegava com 40-60KB de contexto irrelevante.

**A solução:** Adicionar regra ao system prompt:

```
No início de cada sessão, carrega apenas:
1. Ficheiro core de operação
2. Perfil do utilizador
3. Identidade do agente
4. Notas de hoje (se existirem)

Tudo o resto (memória histórica, histórico de conversas,
tool outputs anteriores) carrega sob demanda, só quando necessário.
```

**Sistema de memória:**
- No fim de cada sessão: agente guarda resumo do que foi feito + itens abertos
- Memória organizada em ficheiros diários, não um blob gigante

**Impacto:**
- Antes: 40-60KB por interação
- Depois: 6-10KB por interação
- Redução direta de custo em CADA chamada à API

**Resultado Camada 1:** Overhead de 60% → praticamente zero.

---

## Camada 2 — Gastar Melhor no Trabalho Real (resolve os 25%)

### Fix C: Modelo Certo para a Tarefa Certa

**O problema:** Por defeito, TUDO vai para o modelo premium (Opus/Sonnet 4.5/4.6). 80% das tarefas diárias NÃO precisam de raciocínio avançado.

**A solução:**
- **Haiku como modelo padrão** (10-12x mais barato que Sonnet por chamada, e mais rápido)
- Escalar para Sonnet/Opus APENAS para:
  - Decisões de arquitetura
  - Código de produção
  - Trabalho de segurança
  - Raciocínio multi-step complexo
  - Se não tiver a certeza → começa pelo mais barato

**Configuração:** Atualizar config + adicionar regra de escalamento ao system prompt.

**Impacto:** Custo do trabalho produtivo baixou 70-75%. Tarefas rotineiras também ficam mais rápidas (Haiku é mais ágil).

---

### Fix D: Cortar os Ficheiros de Contexto

**O problema:** Cada linha nos teus ficheiros de configuração (instruções, perfil de utilizador, documentos de referência) é transmitida em CADA chamada à API. Uma biografia de 500 palavras no ficheiro do utilizador = enviada centenas de vezes por dia.

**A solução:**
- Nos ficheiros core: apenas o que o agente precisa para tomar decisões AGORA
  - Princípios operacionais, regras de roteamento, rate limits, nome, fuso horário, projetos ativos
- Todo o resto (contexto detalhado de empresa, histórico, notas) → ficheiros separados que carregam sob demanda

**Tip de Nick (pouco conhecida):**
> Adiciona uma instrução ao ficheiro core para o agente reportar o uso de tokens e custo estimado após cada tarefa. O agente naturalmente fica mais cuidadoso com o gasto — como dar um cartão de empresa e pedir expense reports.

---

### Fix E: Guardrails de Budget

**O problema:** Um loop de automação mal desenhado ou uma tarefa que desencadeia subtarefas pode queimar budget em horas.

**A solução:** Adicionar diretamente ao system prompt do agente:
- Delay mínimo entre chamadas à API
- Máximo de web searches por batch
- Operações similares agrupadas em pedidos únicos
- Limites diários e mensais com alertas antes de atingir

**⚠️ Conselho crítico de Nick:**
> "Until you have run your agent for at least a week with these optimizations in place and you're confident that everything is stable, do not set up automatic billing. Load $5-10 at a time manually and watch how it drains."

Há casos de pessoas que acordaram com centenas de dólares em charges por não fazerem isto.

---

## Camada 3 — O Multiplicador

### Fix F: Prompt Caching

**O mecanismo:**
- Primeira vez que envias contexto → pagas preço cheio
- Chamadas seguintes dentro de uma janela de 5 minutos → **90% de desconto** no conteúdo em cache
- Só pagas preço cheio pela nova mensagem + resposta

**O que deve ser cached (conteúdo estável):**
- Instruções de operação
- Perfil do utilizador
- Documentos de referência

**O que NÃO deve ser cached (conteúdo dinâmico):**
- Notas diárias
- Mensagens recentes
- Tool outputs

**Regra importante:** Não edites as instruções operacionais a meio de uma sessão — quebra o cache.

**Por que se chama "o multiplicador":** O caching não poupa só por si — amplifica TODAS as outras otimizações. Já cortaste o contexto (camada 2), então o conteúdo que fica em cache é leve. Já eliminaste chamadas desnecessárias (camada 1), então não pagas caching em desperdício. Tudo se compõe.

---

## Resultados Finais (Full Stack)

| Camada | Impacto |
|--------|---------|
| Camada 1 (overhead) | Custo idle semanal: $12-15 → ~$0 |
| Camada 2 (work smarter) | Custo por tarefa: -70 a 75% |
| Camada 3 (caching) | Desconto de 90% sobre o conteúdo estável restante |
| **Total** | **$15-20/semana → $0.50-$1/semana** |

Mesmo agente. Mesmas capacidades. Muitas vezes mais rápido.

---

## A Visão Maior (Minuto 16:23)

Nick termina com um insight importante para quem quer oferecer isto como serviço:

> "The people who are actually going to win with AI agents over the next couple of years are not the people who just set them up. They're the people who know how to actually manage them, who understand the cost structure, who can diagnose issues, who can make them run efficiently, all at scale. That is a real skill set and right now almost nobody has it."

**Aplicação ao serviço de implementação:**
- Saber otimizar o OpenClaw é uma vantagem competitiva real agora
- A maioria das pessoas/empresas que instalam ficam com configurações defaults caras
- Oferecer "implementação + otimização de custos" é uma proposta de valor clara e mensurável (ROI direto)

---

## Capítulos do Vídeo

| Timestamp | Capítulo |
|-----------|---------|
| 0:00 | Introduction |
| 1:38 | Part 1: The Audit |
| 3:09 | Heartbeat Problem |
| 4:37 | Context Loading Problem |
| 5:34 | Part 2: Layer One — Stop Invisible Drain |
| 5:46 | Fix A: Redirect Background to Local Model (Ollama/Llama) |
| 7:21 | Fix B: Control Context Loading |
| 8:58 | Part 3: Layer Two — Spend Smarter on Tasks |
| 9:09 | Fix C: Match Model to Job (Haiku default) |
| 10:58 | Fix D: Trim Context Files |
| 12:24 | Fix E: Set Budget Guardrails |
| 14:01 | Part 4: Layer Three — The Multiplier |
| 14:15 | Fix F: Prompt Caching |
| 15:05 | Full Stack Results |
| 16:23 | Part 5: The Bigger Picture |
| 17:35 | Guide + Community Link |

---

## Recursos Mencionados

- **Guia gratuito** com todos os fixes, config templates e checklists: disponível na comunidade Skool de Nick
- **Comunidade Skool:** https://www.skool.com/systems-to-scale-9517
- **Vídeo sobre Ollama:** no canal Nick Puru | AI Automation
- **Canal:** https://www.youtube.com/@NicholasPuru
