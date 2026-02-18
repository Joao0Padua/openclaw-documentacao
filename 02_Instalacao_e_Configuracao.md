# Instalação e Configuração do OpenClaw

> Fonte: docs.openclaw.ai, digitalapplied.com, digitalocean.com

---

## Requisitos do Sistema

| Item | Requisito |
|------|-----------|
| macOS | 12 ou superior |
| Ubuntu | 20.04 ou superior |
| Windows | 10+ com WSL2 |
| Node.js | Versão 22 ou superior |
| RAM | 4GB recomendado |
| Porta | 18789 disponível |

---

## Instalação Rápida

```bash
# Método 1 — Script oficial (mais fácil)
curl -fsSL https://get.openclaw.ai | bash

# Método 2 — via npm
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

O installer cria o diretório `~/.openclaw/` e guia-te pelo setup inicial.

---

## Processo de Onboarding

O assistente de onboarding pede:
1. **Modelo de IA** — Claude (Anthropic), GPT (OpenAI), DeepSeek, ou modelo local (Ollama)
2. **API Key** — Da plataforma escolhida
3. **Canal de mensagens** — WhatsApp, Telegram, Slack, Discord, etc.
4. **Configurações de segurança** — Allowlists, perfis de acesso

Depois de configurar, inicia o daemon:
```bash
openclaw onboard --install-daemon
```

O daemon corre em background automaticamente (launchd no macOS, systemd no Linux).

---

## Canais de Mensagens Suportados

### Principais (mais usados)
| Canal | Notas |
|-------|-------|
| **WhatsApp** | Mais popular. Usa QR code para emparelhar |
| **Telegram** | Ideal para developers, suporta comandos inline |
| **Discord** | Ótimo para equipas e automação em equipa |
| **Slack** | Integração enterprise de workflows |
| **iMessage** | Via app BlueBubbles (só macOS) |
| **Signal** | Foco em privacidade |
| **Microsoft Teams** | Para ambientes empresariais |

### Canais Adicionais
- Google Chat
- Matrix
- Zalo
- Zalo Personal
- Twitch (recente)
- WebChat (interface web própria)

Podes correr **múltiplos canais em simultâneo** sem reconfigurar.

---

## Modelos de IA Suportados

| Modelo | Empresa | Melhor para |
|--------|---------|-------------|
| **Claude Sonnet 4** | Anthropic | Raciocínio e tarefas complexas |
| **GPT-5.2** | OpenAI | Velocidade e código |
| **DeepSeek** | DeepSeek | Custo-benefício |
| **KIMI K2.5** | Moonshot AI | Alternativa asiática |
| **Xiaomi MiMo-V2-Flash** | Xiaomi | Velocidade |
| **Ollama (local)** | — | Privacidade total, offline |

---

## Ficheiro de Configuração

Localização: `~/.openclaw/openclaw.json`

```json
{
  "tools": {
    "profile": "coding",
    "deny": ["group:runtime"]
  },
  "channels": {
    "whatsapp": {
      "enabled": true
    },
    "telegram": {
      "enabled": true
    }
  }
}
```

### Perfis de Acesso a Tools

| Perfil | O que inclui |
|--------|-------------|
| `minimal` | Apenas mensagens e web search |
| `coding` | + execução de código e ficheiros |
| `messaging` | + envio de mensagens cross-platform |
| `full` | Acesso completo a todas as tools |

---

## Segurança de Acesso (Pairing Mode)

Por defeito, mensagens diretas de remetentes desconhecidos requerem um código de emparelhamento:

```bash
# Ver pedidos de emparelhamento pendentes
openclaw pairing list

# Aprovar um remetente
openclaw pairing approve <canal> <codigo>
```

Grupos no Discord/Slack por defeito têm acesso restrito — precisam de estar na allowlist.

---

## Deploy em Docker (Recomendado para Empresas)

```dockerfile
# Exemplo básico
docker run -d \
  --name openclaw \
  -v ~/.openclaw:/app/.openclaw \
  -p 18789:18789 \
  openclaw/openclaw:latest
```

Para produção enterprise: usa Docker Compose com isolamento de rede e rotação de credenciais.

---

## Deploy em Cloud

O OpenClaw pode correr em:
- **DigitalOcean** (tutorial oficial disponível)
- **Fly.io**
- **Google Cloud Platform**
- **Railway**
- **Render**
- **VPS próprio** via SSH/Tailscale

---

## Comandos CLI Essenciais

```bash
openclaw status          # Ver estado do gateway
openclaw --version       # Ver versão instalada
openclaw onboard         # Re-executar onboarding
openclaw pairing list    # Ver pedidos de emparelhamento
```

### Comandos no Chat

```
/status    → Ver estado do sistema
/think     → Modo de raciocínio mais profundo
/verbose   → Ativar logs detalhados
```

---

## Acesso Remoto

Para aceder ao teu OpenClaw de fora de casa:
- **Tailscale** — VPN mesh (recomendado)
- **SSH Tunnel** — Para utilizadores técnicos

⚠️ **Nunca exponhas o gateway (porta 18789) diretamente à internet.**

---

## Fontes

- https://docs.openclaw.ai
- https://www.digitalapplied.com/blog/openclaw-ai-complete-guide-setup-skills-automation
- https://www.digitalocean.com/community/tutorials/how-to-run-openclaw
- https://openclawskill.cc/blog/how-to-install-openclaw-skills
