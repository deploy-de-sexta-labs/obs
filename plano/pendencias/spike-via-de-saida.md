---
type: Spike
title: "Spike: via de saída do programa"
description: "Decidir como levar a cena composta do webview até o FFmpeg: LiveKit Track Egress ou WebCodecs."
tags: [spike, pipeline, webcodecs, track-egress]
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

Como levar a cena composta do webview até o FFmpeg?

# Vias candidatas

1. **LiveKit Track Egress** (sidecar): pega o track do programa e gera RTMP → MediaMTX.
2. **WebCodecs**: encoda com hardware no webview → Rust → FFmpeg apenas muxa (`-c copy`).

# Procedimento

Testar as duas vias e escolher uma. A via escolhida fica atrás de uma interface, para troca barata depois.

# Critério de decisão

Não definido no plano original. Sugestão para discussão: latência, uso de CPU e GPU, e estabilidade em uma transmissão de teste.

# Relationships

- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — onde a decisão se encaixa.
- [Fase 0 — Spike](/roadmap/fase-0-spike.md) — fase em que o spike é feito.
- [Riscos](/riscos.md) — R2.
- [FFmpeg](/componentes/ffmpeg.md) — o papel do FFmpeg depende da via.
