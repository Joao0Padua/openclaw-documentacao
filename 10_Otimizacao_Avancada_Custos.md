# Otimização Avançada de Custos — Guias e Recursos Adicionais

> Compilado de múltiplas fontes: GitHub, blogs técnicos, comunidade OpenClaw
> Data: Fevereiro 2026

---

## Resumo dos Recursos Encontrados

### 1. GitHub: OpenClaw-Token-Optimization (wassupjay)
**URL:** https://github.com/wassupjay/OpenClaw-Token-Optimization

O repositório mais completo sobre otimização de custos encontrado. Promete reduzir de $1.200/mês para ~$50 mantendo performance total.

**Conteúdo:**
- `openclaw.json` — config completo com todas as otimizações
- System prompt templates com todas as regras incorporadas
- Tabelas de troubleshooting para problemas comuns
- Roadmap de implementação em 4 semanas

**Estratégias incluídas:**
```
1. Session Management
   → Carregar apenas SOUL.md, USER.md, IDENTITY.md + notas do dia
   → Redução de ~80% nos custos de startup

2. Model Routing (JSON config)
   → Haiku como padrão para tarefas rotineiras
   → Sonnet apenas para: arquitetura, segurança, raciocínio complexo
   → Poupança: 85-90% nos custos de modelo

3. Local Heartbeats (Ollama)
   → Substitui heartbeats pagos por Ollama local gratuito
   → Elimina ~$50/mês em charges recorrentes

4. Rate Limiting & Budget Controls
   → Delays mínimos entre chamadas à API
   → Throttling de pesquisas web
   → Cap mensal de $180 para evitar surpresas

5. Workspace File Structure
   → Ficheiros estáveis (cacheáveis) vs. dinâmicos separados
   → Maximiza benefícios do prompt caching (90% desconto)

6. Prompt Caching Setup
   → Caching ativo para Sonnet, desativado para Haiku
   → Batching estratégico em janelas de 5 minutos
```

---

### 2. GitHub Gist: "Running OpenClaw Without Burning Money" (digitalknk)
**URL:** https://gist.github.com/digitalknk/ec360aab27ca47cb4106a183b2c25a98

**Filosofia base:**
> Usa um modelo coordenador barato para background, reserva modelos caros para casos específicos e intencionais.

**Estratégias práticas:**
- Sistema de heartbeat rotativo por tipo de tarefa:
  - Email: a cada 30 minutos
  - Calendário: a cada 2 horas
  - Git status: diariamente
- Cap de concorrência para evitar falhas em cascata
- Routing explícito — não auto-routing
- Usar GPT-5 Nano ou equivalente para "background plumbing"
- `openclaw doctor --fix` após mudanças de configuração
- Verificar que gateway só escuta localhost: `netstat -an | grep 18789`

**Custo real do autor com configuração otimizada:** $45-50/mês

**Avisos importantes:**
- Não comprar hardware dedicado no início — aprende primeiro o teu workflow
- Modelos locais raramente poupam dinheiro sem infraestrutura enterprise
- Evitar decisões baseadas em hype

---

### 3. MoltFounders Runbook: Cost Optimization
**URL:** https://moltfounders.com/openclaw-runbook/cost-optimization

**Insight central:**
> "O modelo principal deve ser um coordenador, não um trabalhador."

**Abordagem de model routing:**

| Papel | Modelo |
|-------|--------|
| Coordenador padrão | Claude Sonnet 4.5 (mid-tier) |
| Tarefas premium | Reservado para casos complexos específicos |
| Operações rotineiras | Modelos mais baratos / fallback chain |

**Custo real do autor:** $45-50/mês ($40 subscrições + $5-10 API)

---

### 4. Daily Dose of DS: Cut OpenClaw Costs by 95%
**URL:** https://blog.dailydoseofds.com/p/cut-openclaw-costs-by-95

**Abordagem diferente:** Usar **Minimax M2.5** em vez de Claude/GPT

- Performance: 80.2% no SWE-Bench (equivalente ao Opus 4.6)
- Custo: ~$1/hora com 100 tokens/segundo
- Excelente para tarefas agentic de longa duração

**Setup:**
1. Instalar OpenClaw
2. Selecionar Minimax M2.5 na configuração
3. Plano coding: $8.8/mês (starter)
4. Criar bot Telegram para interação

**Exemplo de equipa de 3 agentes:**
- **Neo** — engenheiro (coding tasks)
- **Pulse** — investigador (research briefings)
- **Pixel** — designer (content creation)

Todos a correr em M2.5 via Telegram.

---

### 5. YingTu: Clawdbot Cost Optimization Guide
**URL:** https://yingtu.ai/blog/clawdbot-cost-optimization-cheap-models

**Modelos recomendados por custo:**

| Modelo | Input | Output | Notas |
|--------|-------|--------|-------|
| Gemini 2.5 Flash | $0.15 | $0.60 | Mais barato mainstream |
| Claude Haiku 4.5 | $1.00 | $5.00 | Melhor custo-benefício Claude |
| Claude Sonnet 4.5 | $3.00 | $15.00 | Performance premium |
| Ollama (local) | $0 | $0 | Privacidade total |

**Haiku 4.5 vs Sonnet 4.5:** 90% da performance de coding a 1/3 do preço.

**Sistema de fallback recomendado:**
```
1º Modelo local (Ollama) — grátis, para tarefas simples
2º Haiku — barato, para tarefas moderadas
3º Sonnet/Opus — só quando estritamente necessário
```

**Controlo de custos:**
- Definir spending caps no dashboard do fornecedor de API
- Resetar sessões regularmente para evitar token bloat
- Evitar acumulação de contexto desnecessário

---

### 6. GitHub Discussion #10234 — Otimizar System Prompt
**URL:** https://github.com/openclaw/openclaw/discussions/10234

**Problema identificado pela comunidade:**
O system prompt padrão do OpenClaw é extremamente longo, especialmente para Heartbeats. Mesmo para tarefas simples carrega context desnecessário.

**Soluções discutidas:**
1. Configuração `lite-mode` em `~/.openclaw/openclaw.json` (a investigar)
2. Plugin Lifecycle Hooks para strip de secções desnecessárias
3. Framework de agente alternativo (mais leve)
4. Offloading para modelos locais como mitigação principal

---

### 7. OpenClaw Official Docs: Context Optimizer Skill
**URL:** https://github.com/openclaw/skills/blob/main/skills/ad2546/context-optimizer/SKILL.md

**Skill oficial de otimização de contexto com 4 estratégias:**

| Estratégia | Descrição |
|------------|-----------|
| Semantic Compaction | Merge de mensagens similares |
| Temporal Compaction | Resumo de conversas antigas por janelas de tempo |
| Extractive Compaction | Extração de info-chave de mensagens verbose |
| Adaptive Compaction | Escolha automática da melhor estratégia |

**Comandos úteis:**
```bash
/context list      # ver o que está a ser injetado no contexto
/context detail    # ver quanto cada ficheiro consome em tokens
```

---

### 8. GitHub Gist: OpenClaw Config Example (Sanitized)
**URL:** https://gist.github.com/digitalknk/4169b59d01658e20002a093d544eb391

Config exemplo limpo com todas as opções de otimização comentadas.

---

### 9. GitHub: Working Setup com Ollama Local
**URL:** https://gist.github.com/Hegghammer/86d2070c0be8b3c62083d6653ad27c23

Setup funcional de OpenClaw com Ollama para quem quer correr tudo localmente sem custos de API.

---

## Comparação de Estratégias de Otimização

| Estratégia | Poupança | Complexidade | Recomendado para |
|------------|---------|--------------|-----------------|
| Haiku como padrão | 75-85% | Baixa | Todos |
| Ollama para heartbeats | ~40% overhead | Média | Quem tem Mac recente |
| Prompt caching | 90% conteúdo estável | Baixa | Todos |
| Context trimming | 60-80% por interação | Média | Utilizadores avançados |
| Minimax M2.5 | 95% vs Claude premium | Baixa | Quem aceita modelo alternativo |
| Gemini 2.5 Flash | 90% vs Claude Sonnet | Baixa | Quem quer mínimo custo cloud |

---

## Config Template Base (openclaw.json otimizado)

```json
{
  "model": {
    "default": "claude-haiku-4-5",
    "routing": {
      "complex_reasoning": "claude-sonnet-4-5",
      "architecture": "claude-sonnet-4-5",
      "security": "claude-sonnet-4-5",
      "code_production": "claude-sonnet-4-5",
      "heartbeat": "ollama/llama3:3b",
      "routine": "claude-haiku-4-5"
    },
    "caching": {
      "enabled": true,
      "providers": ["anthropic"]
    }
  },
  "context": {
    "startup_files": ["SOUL.md", "USER.md", "IDENTITY.md"],
    "load_today_notes": true,
    "load_history_on_demand": true
  },
  "heartbeat": {
    "provider": "local",
    "model": "ollama/llama3:3b",
    "interval_minutes": 60
  },
  "tools": {
    "profile": "coding",
    "deny": ["group:runtime"]
  },
  "budget": {
    "monthly_limit_usd": 50,
    "alert_at_pct": 80,
    "daily_limit_usd": 5
  },
  "rate_limits": {
    "min_delay_ms": 500,
    "max_web_searches_per_batch": 5
  }
}
```

---

## System Prompt Base (otimizado para custo)

```
## Session Startup
On session start, load ONLY:
1. Core operating file (SOUL.md)
2. User profile (USER.md)
3. Agent identity (IDENTITY.md)
4. Today's notes if they exist

Do NOT auto-load: conversation history, memory archives, tool outputs, old notes.
Retrieve these on demand when explicitly needed.

## Model Selection
- Default model: Haiku (fast, cheap, handles 80% of tasks)
- Escalate to Sonnet ONLY for: architecture decisions, production code, security analysis, complex multi-step reasoning
- When unsure: start with Haiku

## Cost Reporting
Before each task: estimate token cost.
After each task: report actual tokens used and estimated cost.

## Budget Controls
- Minimum 500ms delay between API calls
- Maximum 5 web searches per batch
- Alert me if estimated task cost exceeds $0.50
- Batch similar operations into single requests

## Memory Management
End of session: save a concise summary (max 200 words) of what was done and open items.
Organize memory in daily files. Keep files lean — max 500 words each.
```

---

## Fontes

- https://github.com/wassupjay/OpenClaw-Token-Optimization
- https://gist.github.com/digitalknk/ec360aab27ca47cb4106a183b2c25a98
- https://moltfounders.com/openclaw-runbook/cost-optimization
- https://blog.dailydoseofds.com/p/cut-openclaw-costs-by-95
- https://yingtu.ai/blog/clawdbot-cost-optimization-cheap-models
- https://github.com/openclaw/openclaw/discussions/10234
- https://github.com/openclaw/skills/blob/main/skills/ad2546/context-optimizer/SKILL.md
- https://gist.github.com/digitalknk/4169b59d01658e20002a093d544eb391
- https://gist.github.com/Hegghammer/86d2070c0be8b3c62083d6653ad27c23
