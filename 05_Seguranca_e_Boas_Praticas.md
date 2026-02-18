# Segurança e Boas Práticas no OpenClaw

> Fontes: crowdstrike.com, digitalapplied.com, docs.openclaw.ai, o-mega.ai

---

## Riscos Principais

### 1. Configurações Incorretas
- Gateway exposto à internet sem autenticação
- Allowlists muito permissivas em grupos
- Tools com acesso total sem necessidade

### 2. Prompt Injection
- Inputs maliciosos disfarçados de comandos legítimos
- Especialmente perigoso em skills de web scraping
- Um website malicioso pode tentar dar instruções ao agente

### 3. Exposição de Credenciais
- API keys armazenadas em plaintext se mal configuradas
- Skills de terceiros com acesso a vaults de passwords
- Logs que capturam informação sensível

### 4. Supply Chain (Skills Maliciosas)
- Incidente ClawHavoc (Fevereiro 2026): 341 skills maliciosas no ClawHub
- Skills podem ter código executável arbitrário
- Difícil detetar sem revisar o código manualmente

---

## Boas Práticas Essenciais

### Acesso e Permissões
```
✅ Usa contas dedicadas para o agente (não a tua conta pessoal)
✅ Configura allowlists explícitas de remetentes
✅ Usa perfil "minimal" ou "coding" — não "full" — a não ser que necessário
✅ Revoga acesso de skills que já não uses
✅ Separa workspaces por projeto/cliente
```

### Rede e Infraestrutura
```
✅ Nunca exponhas a porta 18789 diretamente à internet
✅ Usa Tailscale ou SSH tunnel para acesso remoto
✅ Para empresas: corre em Docker com isolamento de rede
✅ Rota API keys regularmente
✅ Mantém logs de atividade ativados
```

### Skills e Plugins
```
✅ Verifica sempre o repositório GitHub antes de instalar
✅ Lê o código fonte das skills (é JavaScript/JSON)
✅ Verifica reputação e histórico do autor
✅ Prefere skills com muitas estrelas e reviews
✅ Nunca dás acesso a password vaults a skills não verificadas
```

### Monitorização
```
✅ Ativa o Control UI dashboard para ver atividade em tempo real
✅ Configura alertas para ações sensíveis
✅ Revê logs regularmente, especialmente no início
✅ Testa automações em ambiente controlado antes de produção
```

---

## Configuração Segura Base

```json
{
  "tools": {
    "profile": "coding",
    "deny": ["group:runtime", "exec"]
  },
  "channels": {
    "pairing": {
      "enabled": true,
      "requireCode": true
    },
    "groups": {
      "defaultAccess": "restricted",
      "allowlist": ["@trusted-user-1", "@trusted-user-2"]
    }
  }
}
```

---

## Checklist para Deploy Enterprise

Para clientes empresariais, verificar antes de ir a produção:

- [ ] Docker containerização configurada
- [ ] Contas de serviço dedicadas (não contas pessoais)
- [ ] Credenciais rotativas programadas
- [ ] Audit logging ativo
- [ ] Gateway não exposto publicamente
- [ ] VPN ou Tailscale para acesso remoto
- [ ] Allowlists de remetentes configuradas
- [ ] Todas as skills revisadas e aprovadas
- [ ] Plano de rollback documentado
- [ ] Responsável de segurança designado

---

## O que Diz a CrowdStrike sobre OpenClaw

A CrowdStrike publicou uma análise de segurança identificando os principais vetores de ataque:

1. **Escalonamento de privilégios** via configurações permissivas
2. **Exfiltração de dados** através de skills maliciosas com acesso ao sistema de ficheiros
3. **Lateral movement** em redes empresariais via credenciais armazenadas
4. **Persistência** através do daemon do sistema

**Recomendação da CrowdStrike:** Tratar o processo OpenClaw como qualquer agente com acesso privilegiado — monitorização EDR, logging centralizado, e princípio de mínimo privilégio.

---

## Limitações Atuais da Tecnologia

Ser honesto com clientes sobre estas limitações:

| Limitação | Impacto | Mitigação |
|-----------|---------|-----------|
| Alucinações do modelo | Ações incorretas | Supervisão humana inicial |
| Loops infinitos | Consumo de API | Loop detection (nativo) |
| Interpretação ambígua | Resultado errado | Instruções claras e específicas |
| Skills maliciosas | Segurança | Vetting rigoroso |
| Dependência de API | Downtime | Fallback para modelo local |

---

## Fontes

- https://www.crowdstrike.com/en-us/blog/what-security-teams-need-to-know-about-openclaw-ai-super-agent/
- https://o-mega.ai/articles/openclaw-creating-the-ai-agent-workforce-ultimate-guide-2026
- https://docs.openclaw.ai
- https://www.digitalapplied.com/blog/openclaw-enterprise-automation-business-use-cases-guide
