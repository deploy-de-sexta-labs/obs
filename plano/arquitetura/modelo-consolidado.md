---
type: Architecture
title: "Modelo consolidado"
description: "Princípios do sistema: app instalável, sem servidor dedicado, salas por chave, redundância ativo/standby e papéis das máquinas."
tags: [arquitetura, modelo, papeis]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Princípios

- **App instalável** (Tauri), não via navegador. O mesmo app serve para o **host/diretor** e para o **modo convidado**.
- **Sem servidor dedicado.** O `livekit-server` (SFU) roda na máquina de um dos hosts, gerenciado como *sidecar* pelo app.
- **Salas por chave (multi-tenant).** Ver [Salas por chave](/arquitetura/salas-por-chave.md).
- **Redundância ativo/standby** entre os dois hosts para a saída das plataformas. Ver [Redundância](/arquitetura/redundancia-ativo-standby.md).
- **Convidados são remotos** e **não publicam para as plataformas**: entram apenas na composição.

# Papéis

| Papel | Quem é | Responsabilidade |
|---|---|---|
| Participante | cada host e cada convidado | captura a própria câmera e mic, e envia **um único** stream |
| Transporte (SFU) | `livekit-server` na máquina de um host | encaminha mídia sem recomprimir, resolve NAT e cuida da reconexão |
| Diretor / Programa | máquina que transmite | baixa todos os streams, monta a cena, mixa áudio, encoda 1× e faz fan-out para as 3 plataformas |

# Relationships

- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — como o programa chega às plataformas.
- [LiveKit server](/componentes/livekit-server.md) — implementação do papel de transporte.
- [FFmpeg](/componentes/ffmpeg.md) — implementação do encode e do fan-out.
- [Decisões](/decisoes.md) — D2, D4 e D5 sustentam este modelo.
