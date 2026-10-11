---
type: Stack
title: "Stack"
description: "Tecnologias escolhidas por camada, 100% em Rust, para Windows e Linux: UI, composição, captura, WebRTC, encode e i18n."
tags: [stack, rust, slint, wgpu, livekit, ffmpeg]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Camadas

| Camada | Escolha | Por quê |
|---|---|---|
| Linguagem | **Rust** | Requisito do projeto; núcleo de mídia de alta performance e cross-platform. |
| UI | **Slint** (recomendado) ou **egui/iced** | 100% Rust, sem webview; roda em Windows e Linux. |
| Composição | **`wgpu`** + `cosmic-text`/`glyphon` (+ `resvg`) | Compositor em GPU; D3D12/Vulkan no Windows e Vulkan no Linux. |
| Captura | `xcap`/`scap` (tela), `nokhwa` (câmera), `cpal` + `wasapi`/`libpulse`/`pipewire` (áudio) | Captura nativa por sistema operacional. |
| Convidados | **SDK Rust do LiveKit** (crate `livekit`) ou `webrtc-rs` | WebRTC em Rust, cross-platform. |
| Encode e fan-out | **FFmpeg** (`ffmpeg-next` ou sidecar) + **MediaMTX** | Encode em GPU e distribuição por cópia. |
| Servidor de mídia | `livekit-server` (processo filho) | Transporte dos participantes. |
| i18n | `fluent` (ou gettext do Slint) | PT-BR e EN. |
| Segredos | **`keyring`** | Stream keys no keychain do sistema operacional. |

> **Windows e Linux são o alvo do MVP.** Sem Electron e sem webview — ver [Decisões](/decisoes.md).

# Relationships

- [Decisões](/decisoes.md) — D1 (Rust), D2 (UI em Rust) e D12 (idioma).
- [Captura e composição](/componentes/captura-e-composicao.md) — detalhes do núcleo nativo.
- [UI](/componentes/ui.md) — detalhes da interface.
- [Riscos](/riscos.md) — R2, R4 e R5.
