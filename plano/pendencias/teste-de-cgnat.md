---
type: Spike
title: "Teste de CGNAT e port forwarding"
description: "Verificar se o host recebe um IP público alcançável, condição para o port forwarding dos convidados funcionar."
tags: [spike, rede, cgnat, port-forwarding]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
stale_after: 2026-11-30T23:59:59Z
---

# Pergunta

O provedor do host entrega um IP público alcançável, ou usa CGNAT?

# Por que testar

Port forwarding é a opção sem custo para os convidados chegarem à máquina do host, mas falha sob CGNAT, que é comum hoje. O plano pede para testar isso primeiro.

# Resultados possíveis

| Resultado | Consequência |
|---|---|
| IP público alcançável | Port forwarding é viável para o MVP. |
| CGNAT | Usar relay externo (VPS mínimo ou gerenciado) para o transporte. |

# Relationships

- [Infraestrutura de transporte](/arquitetura/infraestrutura-de-transporte.md) — opções de alcance.
- [Riscos](/riscos.md) — R1.
- [Fase 1 — MVP](/roadmap/fase-1-mvp.md) — depende do resultado.
