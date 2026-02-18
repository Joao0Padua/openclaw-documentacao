# Agenda — Próxima Sessão

> Última actualização: Fevereiro 2026

---

## Documentos a criar

### `12_Seguranca_Avancada.html`
Análise exaustiva de segurança para OpenClaw. Temas:
- Tokens e autenticação (pairing mode, allowlists)
- Exposição de portas — nunca expor 18789 directamente
- Acesso remoto seguro: Tailscale vs SSH tunnel
- Prompt injection — risco em modelos locais pequenos
- Isolamento de agentes (multi-agent routing)
- Segurança em Docker / VPS
- Gestão de credenciais e API keys
- Perfis de acesso a tools (minimal, coding, full)

**Fonte principal:** `docs.openclaw.ai/gateway/security`

---

### `13_Study_Cases_Reais.html` ← DOCUMENTO CRÍTICO
Análise exaustiva de casos de uso reais, do mais simples ao mais complexo.
Destinado a apresentar a potenciais interessados — tem de ser rigoroso, crítico e com custos reais validados.

**Metodologia:**
- Pesquisar casos reais na comunidade OpenClaw (GitHub, fóruns, blogs)
- Validar custos com preços actuais dos serviços
- Ser crítico — incluir limitações, falhas e quando NÃO usar

**Estrutura por nível:**

| Nível | Perfil | Infra | Custo/mês |
|-------|--------|-------|-----------|
| 1 | Uso pessoal simples — assistente WhatsApp/Telegram, lembretes | Mac pessoal / VPS básico | ~$0–7 |
| 2 | Uso pessoal avançado — email + ficheiros + web search, multi-canal | VPS KVM 2 + Ollama | ~$14–22 |
| 3 | Pequena equipa (2–5 pessoas) — produtividade partilhada, Notion, GitHub | VPS KVM 3 / Mac Mini M4 | ~$30–50 |
| 4 | Startup / PME — suporte ao cliente, CRM, Slack, multi-agent | Mac Mini M4 Pro 64GB | ~$50–150 |
| 5 | Empresa / uso intensivo — dezenas de utilizadores, pipelines complexos | Servidor GPU / cluster | ~$200–500+ |

**Formato de cada caso:**
- Problema real
- Solução OpenClaw
- Arquitectura técnica
- Custo detalhado (real, validado)
- Resultado mensurável
- Limitações honestas e o que correu mal

**Tabela comparativa final:** custo vs capacidade vs complexidade de implementação

---

### Mac Mini M4 Pro — Exploração como Solução de Infraestrutura
Integrar na análise de infraestrutura e nos study cases (nível 3–4).

Pontos a cobrir:
- Specs: M4 Pro, até 64GB RAM unificada, Neural Engine
- Velocidade de inferência: 60–120 tok/s com modelos grandes (70B Q4)
- Custo: ~$1.400 (16GB) a ~$2.000 (64GB) — investimento único, sem mensalidade
- Quando compensa vs VPS Hostinger
- Como configurar como servidor OpenClaw local 24/7
- Comparação directa: Mac Mini M4 Pro vs VPS CPU vs VPS GPU

---

## Decisões já fechadas (não reabrir)

| Tema | Decisão |
|------|---------|
| VPS base | Hostinger KVM 2 ($6.99/mês) |
| LLM local | Ollama + Llama 3.1 8B Q4 |
| Context length Ollama | `OLLAMA_CONTEXT_LENGTH=32768` obrigatório |
| GPU no VPS 24/7 | Não — demasiado caro |
| Custo total setup base | ~$14–22/mês |
| Repositório | github.com/Joao0Padua/openclaw-documentacao |

---

## Como retomar a sessão

```bash
# Abrir o documento HTML mais recente no browser
python3 -m http.server 8766 --directory ~/Desktop/OpenClaw_Documentacao/
# → http://localhost:8766/11_VPS_e_LLM_Local.html

# Sincronizar repo
cd ~/Desktop/OpenClaw_Documentacao && git pull
```
