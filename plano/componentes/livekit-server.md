---
type: Sidecar
title: "LiveKit server"
description: "SFU de transporte que encaminha a mídia dos participantes, roda na máquina de um host e emite tokens de sala."
resource: https://livekit.io
tags: [sidecar, sfu, livekit, webrtc]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Papel

- Transporte (SFU): encaminha mídia sem recomprimir, resolve NAT e cuida da reconexão.
- Roda na máquina de um dos hosts. No modelo-alvo, é gerido como **sidecar** pelo app.
- Emite os tokens JWT escopados à sala. O segredo do servidor fica somente na máquina do host.
- Fica no host standby durante a redundância, para que os convidados não caiam se o diretor ativo falhar.

# Execução no MVP

Decisão do responsável: no MVP, o LiveKit roda **local via Docker**, por enquanto. Ver D3 em [Decisões](/decisoes.md).

# Relationships

- [Modelo consolidado](/arquitetura/modelo-consolidado.md) — papel de transporte.
- [Salas por chave](/arquitetura/salas-por-chave.md) — emissão de tokens.
- [Redundância ativo/standby](/arquitetura/redundancia-ativo-standby.md) — SFU no standby.
- [Infraestrutura de transporte](/arquitetura/infraestrutura-de-transporte.md) — alcance dos convidados.
