---
type: Product Vision
title: "Visão geral"
description: "Aplicativo desktop instalável que substitui o OBS Studio no fluxo do podcast Deploy de Sexta, com multistream, convidados remotos e overlays."
tags: [visao, produto, podcast, multistream]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Objetivo

Construir um aplicativo desktop instalável que substitua o OBS Studio para o fluxo do podcast **"Deploy de Sexta"**, com três capacidades centrais:

1. **Multistream nativo** — 1 captura → Twitch + YouTube + Kick simultaneamente.
2. **Convidados remotos** — participantes entram por uma sala WebRTC (estilo Discord), remotamente pela internet, com isolamento por *chave de sala*.
3. **Overlays** — camadas visuais (logo, texto, etc.) sobre o vídeo.

# Contexto

- O podcast é **remoto** (sem local físico); todos os participantes estão na internet.
- **Sem servidor dedicado**: o SFU roda na máquina de um dos hosts, junto com o app.
- Dois hosts podem atuar como **diretor** para **redundância** (a live não cair).

# Resultado do MVP

Um fluxo funcional com **1 host + 1 convidado remoto** transmitindo ao vivo para as 3 plataformas, com overlay e reconexão independente por plataforma.

# Relationships

- [Modelo consolidado](/arquitetura/modelo-consolidado.md) — papéis e princípios do sistema.
- [Fase 1 — MVP](/roadmap/fase-1-mvp.md) — escopo detalhado do primeiro entregável.
- [Decisões](/decisoes.md) — escolhas que sustentam esta visão.
