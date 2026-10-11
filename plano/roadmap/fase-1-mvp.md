---
type: Roadmap Phase
title: "Fase 1 — MVP (1 convidado remoto, Windows e Linux)"
description: "Primeiro entregável: app Rust nativo em Windows e Linux, com 1 convidado remoto, overlays nativos e multistream 3× com reconexão por plataforma."
tags: [roadmap, fase-1, mvp, rust, cross-platform]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Entregas

- App **Rust nativo** para **Windows e Linux**.
- UI 100% Rust (Slint ou egui/iced).
- Captura nativa de câmera, mic e tela; composição em `wgpu`; overlays nativos (logo e texto); mixer de áudio básico.
- **1 convidado** via SDK Rust do LiveKit; sala por chave.
- Multistream 3× via MediaMTX + FFmpeg copy, com **reconexão por plataforma**.
- Painel com status e bitrate por plataforma; botão "Ao vivo".
- i18n PT-BR + EN. Stream keys no keyring.

# Dependências

- Resultado dos spikes de [captura nativa](/pendencias/spike-captura-nativa.md) e [encode](/pendencias/decisao-encode.md), feitos na [Fase 0](/roadmap/fase-0-spike.md).
- Escolha do toolkit de UI (D2 em [Decisões](/decisoes.md)).
- Alcance dos convidados definido após o [teste de CGNAT](/pendencias/teste-de-cgnat.md).
- Servidor de mídia no MVP: LiveKit local (D5 em [Decisões](/decisoes.md)).

# Relationships

- [Visão geral](/visao-geral.md) — resultado esperado do MVP.
- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — fluxo a implementar.
- [Captura e composição](/componentes/captura-e-composicao.md) — núcleo a construir.
- [Infraestrutura de transporte](/arquitetura/infraestrutura-de-transporte.md) — alcance dos convidados.
- [Decisões](/decisoes.md) — D11 define o escopo com 1 convidado.
