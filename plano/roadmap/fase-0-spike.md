---
type: Roadmap Phase
title: "Fase 0 — Spike"
description: "Spike de 1 a 2 dias para validar os ingests, medir a banda de upload e decidir a via de saída do programa."
tags: [roadmap, fase-0, spike]
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

1 a 2 dias.

# Entregas

- Script FFmpeg que captura tela, câmera e áudio e envia para as 3 plataformas.
- Validar os ingests, incluindo o RTMPS do Kick.
- Medir o upload. O multistream consome cerca de 3× o bitrate; confirmar que a internet aguenta.
- Decidir a via de saída: Track Egress ou WebCodecs.

# Relationships

- [Spike: via de saída](/pendencias/spike-via-de-saida.md) — decisão entregue nesta fase.
- [Ingests das plataformas](/componentes/ingests-das-plataformas.md) — validação.
- [Riscos](/riscos.md) — R2 e R3.
- [Fase 1 — MVP](/roadmap/fase-1-mvp.md) — próxima fase.
