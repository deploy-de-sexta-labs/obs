---
type: Spike
title: "Decisão de encode"
description: "Definir se o encode usa o FFmpeg embutido (in-process, ffmpeg-next) ou como processo separado (sidecar)."
tags: [spike, encode, ffmpeg, ffmpeg-next, sidecar]
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

O encode deve rodar **in-process** (crate `ffmpeg-next`) ou o FFmpeg deve ser um **processo separado** (sidecar)?

# Vias candidatas

| Via | Vantagem | Custo |
|---|---|---|
| In-process (`ffmpeg-next`) | Sem processo externo; controle fino | Build e empacotamento mais complexos |
| Processo separado (sidecar) | Isolamento; reutiliza o binário FFmpeg | Enviar frames ao processo |

# Critério de decisão

Não definido no plano. Sugestão para discussão: simplicidade de build e empacotamento em **Windows e Linux**, custo de cópia de frames e robustez.

# Relationships

- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — onde a decisão se encaixa.
- [FFmpeg](/componentes/ffmpeg.md) — componente envolvido.
- [Fase 0 — Spike](/roadmap/fase-0-spike.md) — fase em que o spike é feito.
