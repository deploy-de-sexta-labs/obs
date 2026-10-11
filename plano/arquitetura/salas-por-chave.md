---
type: Architecture
title: "Salas por chave"
description: "Isolamento multi-tenant: cada chave de sala dá acesso a uma única sala, e o segredo do SFU fica na máquina do host."
tags: [arquitetura, seguranca, salas, multi-tenant, jwt]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Funcionamento

- O host abre a sessão, e o app gera ou usa um **room key**.
- O `livekit-server` local emite **tokens JWT escopados à sala**: só quem tem a chave recebe token.
- **Chave = sala = isolamento total.** Outros podcasts usam o mesmo app, cada um com seu SFU e sua sala.
- O segredo do `livekit-server` fica somente na máquina do host.

# Relationships

- [Decisões](/decisoes.md) — D6 define o convidado instalando o app e entrando com a chave.
- [LiveKit server](/componentes/livekit-server.md) — emissor dos tokens.
- [Modelo consolidado](/arquitetura/modelo-consolidado.md) — princípio de multi-tenant.
