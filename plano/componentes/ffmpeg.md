---
type: Sidecar
title: "FFmpeg"
description: "Motor de encode com aceleração de GPU e de envio às plataformas por cópia, com reconexão independente para cada destino."
resource: https://ffmpeg.org
tags: [sidecar, ffmpeg, encode, nvenc, fan-out]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
stale_after: 2026-11-30T23:59:59Z
---

# Papel

- **Encode 1×** com NVENC ou QSV; **VAAPI** no Linux e **AMF** no Windows; fallback para libx264.
- **Fan-out** por processos separados com `-c copy`, um por plataforma, cada um com reconexão própria.

# Como o app fala com o FFmpeg

Decisão em aberto: **in-process** (`ffmpeg-next`) ou **processo separado** (sidecar). Ver [Decisão de encode](/pendencias/decisao-encode.md).

# Por que processos separados no fan-out

Em vez de um único `tee`, cada plataforma tem seu próprio processo. Assim, uma queda em um destino não derruba os outros.

# Relationships

- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — encode e fan-out no fluxo.
- [Decisão de encode](/pendencias/decisao-encode.md) — in-process vs sidecar.
- [Ingests das plataformas](/componentes/ingests-das-plataformas.md) — destinos.
- [Alvo de resolução e bitrate](/pendencias/alvo-de-resolucao-e-bitrate.md) — parâmetros de encode.
