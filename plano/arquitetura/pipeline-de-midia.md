---
type: Architecture
title: "Pipeline de mídia"
description: "Fluxo da cena composta no webview até as três plataformas, com encode único e fan-out por cópia."
tags: [midia, pipeline, ffmpeg, mediamtx, livekit, webview]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
stale_after: 2026-11-30T23:59:59Z
---

# Fluxo

Como há convidados desde o MVP, os tracks WebRTC chegam no webview. Por isso, a **composição acontece no webview** (canvas + overlays HTML/CSS).

```
WebView2 (canvas): host cam + guest cam + overlays + mix de áudio
        │  (a cena composta é publicada como 1 track WebRTC na sala LiveKit local)
        ▼
livekit-server (sidecar)  ──►  saída do programa
        ▼
encode 1× (GPU: NVENC/QSV/AMF, fallback libx264)
        ▼
MediaMTX local (rtmp://localhost/live)
        ├─ FFmpeg copy ─► Twitch    (reconnect próprio)
        ├─ FFmpeg copy ─► YouTube   (reconnect próprio)
        └─ FFmpeg copy ─► Kick      (RTMPS, reconnect próprio)
```

O diagrama segue a via A (abaixo), com o encode depois do `livekit-server`. Na via B, o encode acontece no próprio webview.

# Variantes de saída (spike pendente)

A forma de levar a cena do webview até o FFmpeg ainda será testada. As duas vias candidatas são:

- **Via A — LiveKit Track Egress (sidecar):** o track do programa é exportado pelo sidecar e gerado em RTMP → MediaMTX. Reaproveita o LiveKit.
- **Via B — WebCodecs:** o webview encoda com hardware → Rust → FFmpeg apenas muxa (`-c copy`).

A via escolhida fica **atrás de uma interface**, para permitir troca barata depois. Ver [Spike: via de saída](/pendencias/spike-via-de-saida.md).

# Por que relay + fan-out com `-c copy`

- **Encode único** (barato, em GPU).
- **Cada plataforma independente:** uma queda não derruba as outras. Isso difere do `tee`, em que todas as saídas dependem do mesmo processo.
- O relay local é também o **ponto natural** onde o host standby assume no failover.

# Relationships

- [MediaMTX](/componentes/mediamtx.md) — relay local.
- [FFmpeg](/componentes/ffmpeg.md) — encode e fan-out.
- [LiveKit server](/componentes/livekit-server.md) — transporte e saída do programa.
- [Ingests das plataformas](/componentes/ingests-das-plataformas.md) — destinos do fan-out.
- [Redundância ativo/standby](/arquitetura/redundancia-ativo-standby.md) — uso do relay no failover.
