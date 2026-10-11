---
type: Architecture
title: "Pipeline de mídia"
description: "Fluxo nativo em Rust: captura por sistema operacional, composição em wgpu, encode único e fan-out por cópia para as três plataformas."
tags: [midia, pipeline, rust, wgpu, ffmpeg, mediamtx]
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

A captura e a composição são **nativas em Rust** (sem webview). Os tracks dos convidados chegam pelo SDK Rust do LiveKit e entram na composição junto com as fontes locais.

```
Núcleo de mídia (Rust)
 ├ Captura: câmera, tela e áudio (nativo por sistema operacional)
 └ Convidados: tracks WebRTC via SDK Rust do LiveKit
        ▼
Composição (wgpu): layout + overlays (texto/imagem/SVG) + mix de áudio
        ▼
encode 1× (GPU: NVENC/QSV; VAAPI no Linux, AMF no Windows; fallback libx264)
        ▼
MediaMTX local (rtmp://localhost/live)
        ├─ FFmpeg copy ─► Twitch    (reconnect próprio)
        ├─ FFmpeg copy ─► YouTube   (reconnect próprio)
        └─ FFmpeg copy ─► Kick      (RTMPS, reconnect próprio)
```

# Decisão em aberto

Como a cena composta chega ao FFmpeg ainda será decidido: **in-process** (`ffmpeg-next`) ou **FFmpeg como processo separado** (sidecar). Ver [Decisão de encode](/pendencias/decisao-encode.md).

# Por que relay + fan-out com `-c copy`

- **Encode único** (barato, em GPU).
- **Cada plataforma independente:** uma queda não derruba as outras. Isso difere do `tee`, em que todas as saídas dependem do mesmo processo.
- O relay local é também o **ponto natural** onde o host standby assume no failover.

# Relationships

- [Captura e composição](/componentes/captura-e-composicao.md) — o núcleo nativo.
- [MediaMTX](/componentes/mediamtx.md) — relay local.
- [FFmpeg](/componentes/ffmpeg.md) — encode e fan-out.
- [Ingests das plataformas](/componentes/ingests-das-plataformas.md) — destinos do fan-out.
- [Redundância ativo/standby](/arquitetura/redundancia-ativo-standby.md) — uso do relay no failover.
