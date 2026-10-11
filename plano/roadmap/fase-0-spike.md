---
type: Roadmap Phase
title: "Fase 0 — Spike"
description: "Validar o núcleo nativo: captura de tela, câmera e áudio em Windows e Linux, encode e fan-out, medicação de upload e decisão de saída."
tags: [roadmap, fase-0, spike, captura, rust]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
stale_after: 2026-11-30T23:59:59Z
---

# Duração

Curto, focado em provar o núcleo nativo. O esforço é maior que o de um spike webview, porque há duas plataformas.

# Entregas

- Captura nativa de **tela, câmera e áudio** funcionando em **Windows e Linux** (incluindo Wayland).
- Pipeline de **encode → MediaMTX → 3 plataformas**.
- Medir o upload. O multistream consome cerca de 3× o bitrate; confirmar que a internet aguenta.
- Validar os ingests, incluindo o RTMPS do Kick.
- Decidir a saída: FFmpeg **in-process** ou **processo separado**.

# Relationships

- [Spike: captura nativa](/pendencias/spike-captura-nativa.md) — o risco principal desta fase.
- [Decisão de encode](/pendencias/decisao-encode.md) — in-process vs sidecar.
- [Ingests das plataformas](/componentes/ingests-das-plataformas.md) — validação.
- [Riscos](/riscos.md) — R2, R3 e R4.
- [Fase 1 — MVP](/roadmap/fase-1-mvp.md) — próxima fase.
