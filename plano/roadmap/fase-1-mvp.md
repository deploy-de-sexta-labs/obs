---
type: Roadmap Phase
title: "Fase 1 — MVP (1 convidado remoto)"
description: "Primeiro entregável: app Tauri instalável com host, 1 convidado remoto, overlay, mixer básico e multistream 3× com reconexão por plataforma."
tags: [roadmap, fase-1, mvp]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Entregas

- App Tauri instalável (Windows).
- Host com câmera, mic e tela. **1 convidado** via LiveKit (sidecar + TURN). Sala por chave.
- Cena com host + convidado.
- Overlay de logo e texto. Mixer de áudio básico.
- Multistream 3× via MediaMTX + FFmpeg copy, com reconexão por plataforma.
- Painel com status e bitrate por plataforma; botão "Ao vivo".
- i18n PT-BR + EN. Stream keys no keychain.

# Dependências

- Resultado do [spike de via de saída](/pendencias/spike-via-de-saida.md), feito na [Fase 0](/roadmap/fase-0-spike.md).
- Alcance dos convidados definido após o [teste de CGNAT](/pendencias/teste-de-cgnat.md).
- Servidor de mídia no MVP: LiveKit via Docker (D3 em [Decisões](/decisoes.md)).

# Relationships

- [Visão geral](/visao-geral.md) — resultado esperado do MVP.
- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — fluxo a implementar.
- [Infraestrutura de transporte](/arquitetura/infraestrutura-de-transporte.md) — alcance dos convidados.
- [Decisões](/decisoes.md) — D8 define o escopo com 1 convidado.
