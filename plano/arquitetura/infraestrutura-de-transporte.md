---
type: Infrastructure
title: "Infraestrutura de transporte"
description: "Alcance dos convidados pela internet sem servidor dedicado: port forwarding, relay TURN ou VPS mínimo, com o app flexível para escolher."
tags: [infraestrutura, rede, nat, cgnat, turn, vps]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: review-needed
stale_after: 2026-11-30T23:59:59Z
---

# Conflito

"Sem servidor dedicado" e "convidados pela internet" se **chocam no nível de rede**. O WebRTC precisa de um endereço público alcançável.

# Opções

| Opção | Vantagem | Limitação |
|---|---|---|
| Port forwarding no roteador do host | Sem custo | Falha sob CGNAT, que é comum hoje. Testar primeiro. |
| Relay TURN público | Funciona atrás de NAT simétrico | Também exige IP público. |
| VPS mínimo (~US$5/mês) só para TURN ou SFU | Custo baixo; composição e encode continuam locais | Adiciona um servidor ao setup. |

# Decisão adotada

App **flexível**: funciona com SFU e TURN locais ou apontando para um relay externo (VPS mínimo ou gerenciado), mantendo composição e encode na máquina local.

Status: aguarda o [teste de CGNAT](/pendencias/teste-de-cgnat.md) para confirmar qual opção o host vai usar.

# Relationships

- [Riscos](/riscos.md) — R1.
- [Decisões](/decisoes.md) — D2 e D7.
- [Teste de CGNAT](/pendencias/teste-de-cgnat.md) — pendência que define a escolha.
