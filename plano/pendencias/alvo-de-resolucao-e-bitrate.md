---
type: Open Question
title: "Alvo de resolução e bitrate"
description: "Definir resolução, FPS e bitrate por plataforma, que determinam a banda de upload e a carga de encode."
tags: [pendencia, bitrate, resolucao, encode]
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

Qual o alvo de resolução, FPS e bitrate para cada plataforma?

# Referência atual

O plano cita, como exemplo, 1080p30 com cerca de 6 Mbps por plataforma. Ainda não há decisão.

# Impacto

O multistream consome cerca de 3× o bitrate no upload. Com 6 Mbps por plataforma, são cerca de 18 Mbps de upload. Ver [Riscos](/riscos.md), R3.

# Relationships

- [FFmpeg](/componentes/ffmpeg.md) — encode.
- [Fase 0 — Spike](/roadmap/fase-0-spike.md) — medição de upload.
