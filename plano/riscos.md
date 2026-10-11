---
type: Risk Register
title: "Riscos e mitigações"
description: "Riscos identificados no plano, com a mitigação prevista e os concepts afetados."
tags: [riscos, mitigacao]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
stale_after: 2026-11-30T23:59:59Z
---

# Registro

| # | Risco | Mitigação | Ver |
|---|---|---|---|
| R1 | CGNAT e alcance dos convidados | App flexível: SFU/TURN local ou relay externo. | [Infraestrutura de transporte](/arquitetura/infraestrutura-de-transporte.md), [Teste de CGNAT](/pendencias/teste-de-cgnat.md) |
| R2 | Levar a cena do webview até o FFmpeg | Spike das duas vias (Track Egress e WebCodecs), atrás de uma interface. | [Spike: via de saída](/pendencias/spike-via-de-saida.md) |
| R3 | Banda de upload 3× (≈18 Mbps para 6 Mbps) | Medir na Fase 0. | [Fase 0 — Spike](/roadmap/fase-0-spike.md), [Alvo de resolução e bitrate](/pendencias/alvo-de-resolucao-e-bitrate.md) |
| R4 | getDisplayMedia e WebCodecs no WebView2 | Testar cedo; se falhar, fallback de captura nativa via Rust. | [Stack](/componentes/stack.md) |
| R5 | Segurança das stream keys | Armazenar no keychain do SO. | [Stack](/componentes/stack.md) |

# Relationships

- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — origem de R2.
- [Ingests das plataformas](/componentes/ingests-das-plataformas.md) — contexto de R3.
