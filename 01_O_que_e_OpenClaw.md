# O que é o OpenClaw?

> Documentação compilada em: Fevereiro 2026
> Fonte principal: openclaw.ai, github.com/openclaw/openclaw, docs.openclaw.ai

---

## Definição

OpenClaw é um **agente de IA pessoal open-source** que corre localmente na tua máquina (Mac, Windows ou Linux). Em vez de ser apenas um chatbot, é um agente que **executa tarefas reais** de forma autónoma.

Criado por Peter Steinberger (que em Fevereiro 2026 anunciou que se junta à OpenAI, passando o projeto para uma fundação open-source).

> "ChatGPT é uma ferramenta de chat. OpenClaw é um agente. A diferença está no que acontece depois da conversa: o ChatGPT só fala contigo, enquanto o OpenClaw age."

---

## História e Nomes

O projeto passou por três nomes antes de se chamar OpenClaw:
1. **Clawdbot** — nome original
2. **Moltbot** — segundo nome
3. **OpenClaw** — nome atual (captura o espírito "open source, open to everyone" + referência ao mascote lagosta 🦞)

**Marcos importantes:**
- Criado em finais de 2025
- Ganhou 60,000+ estrelas no GitHub em 72 horas
- Em Fevereiro 2026 já tem mais de 100,000 estrelas no GitHub
- ClawHub (marketplace de skills) já conta com 3,286 skills com 1.5M+ downloads

---

## Como Funciona

OpenClaw combina:
1. **Um grande modelo de linguagem (LLM)** — à tua escolha: Claude, GPT, DeepSeek, Ollama (local), etc.
2. **Skills/Tools** — integrações e plugins que dão ao AI "mãos" para agir no mundo real
3. **Gateway local** — processo que corre na tua máquina, coordenando tudo

Tu interages via apps de mensagens (WhatsApp, Telegram, Slack, Discord...) e o OpenClaw traduz os teus pedidos em ações reais na tua máquina ou servidor.

---

## Arquitetura Principal

```
[Tu via WhatsApp/Telegram/Slack...]
          ↓
    [Gateway Local]  ← WebSocket em ws://127.0.0.1:18789
          ↓
    [Modelo de IA]   ← Claude / GPT / DeepSeek / Ollama
          ↓
    [Tools & Skills] ← Browser, ficheiros, email, calendário...
          ↓
    [Ação no mundo real]
```

**Componentes:**
- **Gateway** — Hub WebSocket central que coordena sessões, canais, ferramentas e eventos
- **Channels** — Integrações com apps de mensagens (50+)
- **Tools** — Capacidades nativas (browser, exec, ficheiros, web_search...)
- **Skills** — Plugins da comunidade que estendem funcionalidades
- **Nodes** — Apps companion para macOS/iOS/Android (capturas de ecrã, comandos locais)

---

## Princípios Base

- **Local-first** — Os teus dados não saem da tua rede
- **Open-source** — MIT License, código totalmente aberto
- **Bring Your Own API Key** — Tu escolhes e pagas o teu próprio modelo de IA
- **Multi-canal** — Um único sistema, acessível por múltiplas plataformas
- **Extensível** — Skills da comunidade + skills personalizadas

---

## Fontes

- https://openclaw.ai
- https://github.com/openclaw/openclaw
- https://docs.openclaw.ai
- https://en.wikipedia.org/wiki/OpenClaw
- https://www.digitalocean.com/resources/articles/what-is-openclaw
