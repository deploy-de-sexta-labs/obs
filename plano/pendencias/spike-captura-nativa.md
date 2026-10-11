---
type: Spike
title: "Spike: captura nativa"
description: "Validar captura de tela, câmera e áudio nas APIs nativas de Windows e Linux, incluindo Wayland."
tags: [spike, captura, wayland, x11, wasapi, pipewire]
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

A captura de tela, câmera e áudio funciona de forma confiável em **Windows e Linux**, incluindo **Wayland**?

# Vias candidatas

1. **Crates cross-platform**: `xcap`/`scap` (tela), `nokhwa` (câmera), `cpal` (áudio).
2. **APIs diretas por sistema operacional**: DXGI/WGC e WASAPI no Windows; PipeWire/X11, V4L2 e PulseAudio no Linux.

# Procedimento

Testar as duas vias nas duas plataformas e escolher uma. A captura fica **abstraída atrás de uma interface**, por sistema operacional.

# O que observar

- Wayland exige PipeWire e portal; X11 usa XShm.
- Áudio do sistema (loopback) tem APIs diferentes por SO.
- Hotplug de câmera e de monitor.

# Relationships

- [Captura e composição](/componentes/captura-e-composicao.md) — o núcleo validado aqui.
- [Fase 0 — Spike](/roadmap/fase-0-spike.md) — fase em que o spike é feito.
- [Riscos](/riscos.md) — R2 e R4.
