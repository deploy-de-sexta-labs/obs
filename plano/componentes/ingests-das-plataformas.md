---
type: Platform Integration
title: "Ingests das plataformas"
description: "Endpoints de publicação RTMP e RTMPS de Twitch, YouTube e Kick, com a regra de publicador único por stream key."
tags: [plataformas, ingest, rtmp, rtmps, twitch, youtube, kick]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
stale_after: 2026-11-30T23:59:59Z
---

# Schema

| Plataforma | Protocolo | Endpoint de ingest |
|---|---|---|
| Twitch | RTMP | `rtmp://live.twitch.tv/app/<stream_key>` |
| YouTube | RTMP | `rtmp://a.rtmp.youtube.com/live2/<stream_key>` |
| Kick | RTMPS | `rtmps://fa723fc1b171.global-contribute.live-video.net/app/<stream_key>` |

# Notas

- As três plataformas aceitam **um único publicador** por stream key. Ver [Redundância](/arquitetura/redundancia-ativo-standby.md).
- O Kick usa RTMPS, ou seja, RTMP com TLS.
- Os endpoints devem ser validados na [Fase 0](/roadmap/fase-0-spike.md), incluindo o RTMPS do Kick.
- As stream keys ficam no keychain do SO (ver [Stack](/componentes/stack.md)).

# Relationships

- [Redundância ativo/standby](/arquitetura/redundancia-ativo-standby.md) — restrição de publicador único.
- [FFmpeg](/componentes/ffmpeg.md) — envia para cada destino.
