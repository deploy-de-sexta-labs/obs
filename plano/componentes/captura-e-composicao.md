---
type: Component
title: "Captura e composição"
description: "Núcleo nativo de mídia em Rust: captura de tela, câmera e áudio por sistema operacional, e composição da cena em wgpu."
tags: [componente, captura, composicao, wgpu, audio]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
stale_after: 2026-11-30T23:59:59Z
---

# Captura por sistema operacional

| Fonte | Windows | Linux | Crates sugeridos |
|---|---|---|---|
| Tela | DXGI/Desktop Duplication ou Windows.Graphics.Capture | PipeWire (Wayland) ou X11 (XShm) | `xcap` / `scap` |
| Câmera | Media Foundation | V4L2 | `nokhwa` |
| Áudio (mic) | WASAPI | PulseAudio/PipeWire | `cpal` |
| Áudio (sistema) | WASAPI loopback | PulseAudio/PipeWire | `wasapi` / `libpulse` / `pipewire` |

A captura é abstraída por sistema operacional, atrás de uma interface única.

# Composição

- **`wgpu`** monta a cena: layout das fontes, overlays e mistura.
- Texto e formas com `cosmic-text`/`glyphon`; SVG com `resvg`.
- **Overlays** (logo, texto) são renderizados aqui, já que não existe HTML/CSS.

# Relação com o resto do pipeline

A cena composta segue para o encode e o fan-out. Ver [Pipeline de mídia](/arquitetura/pipeline-de-midia.md).

# Relationships

- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — posição no fluxo.
- [Spike: captura nativa](/pendencias/spike-captura-nativa.md) — validação nas duas plataformas.
- [Riscos](/riscos.md) — R2 e R4.
