# Tools e Skills do OpenClaw

> Fonte: docs.openclaw.ai/tools, docs.openclaw.ai/tools/skills, yu-wenhao.com

---

## Distinção Importante

> **Skills** são apenas manuais de instruções. As **capacidades reais** são controladas por `tools.allow`.

- **Tools** = capacidades nativas do sistema (o que o agente pode fazer)
- **Skills** = plugins/instruções que ensinam o agente a usar as tools de forma especializada

---

## Tools Nativas (25 Tools Oficiais)

### Execução e Sistema

| Tool | Descrição |
|------|-----------|
| `exec` | Executa comandos shell com suporte a background |
| `process` | Gere processos em background (list, poll, log, kill) |

### Ficheiros

| Tool | Descrição |
|------|-----------|
| `read` | Lê ficheiros do sistema |
| `write` | Escreve/cria ficheiros |
| `edit` | Edita ficheiros existentes |
| `apply_patch` | Aplica patches a ficheiros |

### Web e Automação

| Tool | Descrição |
|------|-----------|
| `browser` | Controla instância de browser (snapshots, screenshots, ações UI) |
| `web_search` | Pesquisa web via Brave Search API |
| `web_fetch` | Extrai conteúdo de URLs |
| `canvas` | Drive visual canvas presentations (A2UI) |

### Comunicação

| Tool | Descrição |
|------|-----------|
| `message` | Envia mensagens via Discord, Slack, Teams, WhatsApp, iMessage, Telegram, Signal |
| `sessions_*` | Interage com outras sessões de chat |

### Sistema e Infraestrutura

| Tool | Descrição |
|------|-----------|
| `nodes` | Gere nodes macOS/headless; captura câmera/ecrã |
| `cron` | Agenda jobs no gateway |
| `gateway` | Gere processo do Gateway, atualizações |

---

## Grupos de Tools (Atalhos de Configuração)

```json
"group:fs"          → Ferramentas de ficheiros (read, write, edit, apply_patch)
"group:runtime"     → Execução (exec, process)
"group:browser"     → Browser e web
"group:messaging"   → Todas as tools de mensagens
"group:nodes"       → Gestão de nodes/dispositivos
```

---

## Skills — O Ecossistema

### O que são Skills

Skills são **plugins extensíveis** que ensinam o agente a usar as tools de forma especializada. Podem ser:
- **Bundled** — Incluídas por defeito
- **Managed** — Do ClawHub (marketplace oficial)
- **Workspace-specific** — Criadas por ti para casos específicos

### ClawHub — Marketplace de Skills

- **URL:** https://clawhub.biz
- **Total:** 3,286+ skills
- **Downloads:** 1.5M+

#### Categorias de Skills no ClawHub

| Categoria | Nº de Skills |
|-----------|-------------|
| AI/ML | 1,588 |
| Utility | 1,520 |
| Development | 976 |
| Productivity | 822 |
| Communication | ~600 |
| Home Automation | ~400 |
| Finance | ~300 |
| Content Creation | ~250 |
| Health & Wellness | ~200 |
| Research | ~150 |
| Other | ~200 |

### Como Instalar Skills

#### Via Interface Web (Recomendado)
1. Abre o dashboard OpenClaw no browser (`http://localhost:18789`)
2. Vai à secção **Skills**
3. Pesquisa a skill que queres
4. Clica em **Install**
5. Preenche campos de configuração (tokens, endpoints, IDs)
6. Guarda e reinicia se necessário

#### Via CLI
```bash
openclaw skills install <nome-da-skill>
openclaw skills list
openclaw skills remove <nome-da-skill>
```

### Skills Mais Populares (Exemplos)

| Skill | Categoria | O que faz |
|-------|-----------|-----------|
| Gmail Manager | Produtividade | Lê, categoriza, responde emails |
| Google Calendar | Produtividade | Gere eventos e agenda |
| GitHub Integration | Development | PRs, issues, commits |
| Slack Notifier | Comunicação | Envia alertas para Slack |
| Web Scraper | Utility | Extrai dados de websites |
| Spotify Controller | Entertainment | Controla reprodução de música |
| Home Assistant | Home Auto | Controla dispositivos smart home |
| Obsidian Notes | Produtividade | Integra com Obsidian |
| Twitter/X Poster | Social | Publica tweets automaticamente |
| Report Generator | Business | Gera relatórios de dados |

---

## Configuração de Segurança das Tools

```json
{
  "tools": {
    "profile": "coding",
    "allow": ["web_search", "web_fetch", "browser"],
    "deny": ["exec", "group:runtime"]
  },
  "providers": {
    "anthropic": {
      "tools": {
        "deny": ["exec"]
      }
    }
  }
}
```

### Funcionalidades de Segurança

- **Loop detection** — Identifica padrões repetitivos sem progresso para evitar loops infinitos
- **Provider-specific policies** — Restringe tools por modelo de IA sem alterar defaults globais
- **Per-agent overrides** — Configurações diferentes por agente/workspace
- **Multi-instance browser** — Suporte para até ~100 perfis de browser diferentes

---

## ⚠️ Aviso de Segurança — ClawHavoc

Em Fevereiro 2026, o incidente **ClawHavoc** revelou **341 skills maliciosas** no ClawHub que distribuíam malware.

**Boas práticas antes de instalar qualquer skill:**
1. Verifica o repositório GitHub da skill
2. Lê o código fonte (é só JavaScript/JSON)
3. Verifica a reputação do autor
4. Usa contas dedicadas com permissões limitadas
5. Revoga o acesso de skills que já não uses

---

## Fontes

- https://docs.openclaw.ai/tools
- https://docs.openclaw.ai/tools/skills
- https://yu-wenhao.com/en/blog/openclaw-tools-skills-tutorial/
- https://clawhub.biz
- https://nwosunneoma.medium.com/setting-up-skills-in-openclaw-d043b76303be
