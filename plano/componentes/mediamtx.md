---
type: Sidecar
title: "MediaMTX"
description: "Relay RTMP local que recebe o programa codificado e serve de origem para os processos de fan-out."
resource: https://github.com/bluenviron/mediamtx
tags: [sidecar, rtmp, relay, mediamtx]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Papel

- Relay local em `rtmp://localhost/live`, gerido como **sidecar** pelo app.
- Recebe o programa codificado uma única vez.
- Alimenta três processos FFmpeg com `-c copy`, um por plataforma, cada um com reconexão própria.
- É o ponto natural onde o host standby assume a publicação no failover.

# Relationships

- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — posição do relay no fluxo.
- [FFmpeg](/componentes/ffmpeg.md) — processos de fan-out que consomem o relay.
- [Redundância ativo/standby](/arquitetura/redundancia-ativo-standby.md) — troca de publicador.
