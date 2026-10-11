---
type: Stack
title: "Stack"
description: "Tecnologias escolhidas por camada, com o motivo de cada uma: Tauri, React, WebView2, LiveKit, FFmpeg, MediaMTX, i18next e keychain."
tags: [stack, tauri, react, webview2, i18n]
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
| Shell | **Tauri v2** (Rust) | App leve; backend Rust ideal para orquestrar binários sidecar. |
| Frontend | **React + TypeScript + Tailwind** | Reaproveita o conhecimento do projeto; overlays em HTML/CSS. |
| Webview | **WebView2** (Windows) | Chromium: WebRTC, `getDisplayMedia` e WebCodecs. |
| Sidecars (geridos pelo Rust) | `ffmpeg`, `mediamtx`, `livekit-server` | Encode e fan-out, relay local e SFU. |
| SDK de mídia | `livekit-client` (JS no webview) | Publicar e assinar tracks, salas e reconexão. |
| Empacotamento | Tauri bundler (instalador Windows primeiro) | Gera `.msi` e `.exe`. |
| i18n | **i18next** | PT-BR e EN. |
| Segredos | **keychain do SO** | Stream keys nunca em texto puro. |

> Windows é o alvo primário. macOS e Linux ficam para depois, porque o WebView usa WebKit nessas plataformas, o que pode exigir fallbacks de captura.

# Relationships

- [Decisões](/decisoes.md) — D1 (Tauri, não Electron) e D9 (idioma).
- [Riscos](/riscos.md) — R4 e R5.
- [Sidecars](/componentes/index.md) — binários geridos pelo Rust.
