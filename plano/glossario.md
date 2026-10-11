---
type: Glossary
title: "Glossário"
description: "Termos de transporte, mídia, captura e composição usados no plano."
tags: [glossario, referencia]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Termos

| Termo | Definição |
|---|---|
| **App nativo** | Aplicativo compilado para o sistema operacional, sem navegador embutido. |
| **Rust** | Linguagem do aplicativo e do núcleo de mídia. |
| **Slint / egui / iced** | Toolkits de interface em Rust, sem webview. |
| **wgpu** | Camada de GPU do compositor. Usa D3D12 ou Vulkan no Windows e Vulkan no Linux. |
| **Compositor** | Monta a cena final em GPU: layout, overlays e mix de áudio. |
| **Captura nativa** | Leitura de tela, câmera e áudio pelas APIs do próprio sistema operacional. |
| **DXGI / Desktop Duplication** | API do Windows para capturar a tela. |
| **Windows.Graphics.Capture** | API moderna do Windows para capturar telas e janelas. |
| **Wayland** | Servidor gráfico Linux moderno; captura via PipeWire e portal. |
| **X11** | Servidor gráfico Linux legado; captura via XShm. |
| **PipeWire** | Servidor de mídia do Linux usado na captura (Wayland) e no áudio. |
| **V4L2** | API de câmera do Linux. |
| **WASAPI** | API de áudio do Windows; o modo loopback captura o áudio do sistema. |
| **PulseAudio** | Servidor de áudio do Linux. |
| **VAAPI** | Codificação por hardware no Linux (Intel e AMD). |
| **AMF** | Codificação por hardware da AMD no Windows. |
| **NVENC / QSV** | Codificação por hardware de NVIDIA e Intel, disponível nas duas plataformas. |
| **libx264** | Codificador H.264 por software, usado como fallback. |
| **Sidecar** | Processo externo (FFmpeg, MediaMTX, livekit-server) iniciado pelo app. |
| **SFU** (Selective Forwarding Unit) | Servidor que recebe o stream de cada participante e o encaminha aos demais sem recomprimir. |
| **WebRTC** | Tecnologia de comunicação em tempo real (áudio, vídeo e dados). |
| **webrtc-rs / SDK Rust do LiveKit** | Implementações de WebRTC em Rust usadas para os convidados. |
| **STUN** | Servidor que ajuda um participante a descobrir seu endereço público; não retransmite mídia. |
| **TURN** | Servidor que retransmite a mídia quando a conexão direta não é possível. |
| **NAT simétrico** | Tipo de NAT que costuma impedir a conexão direta entre pares, exigindo TURN. |
| **CGNAT** | Carrier-Grade NAT: o provedor compartilha um IP público entre vários clientes, impedindo port forwarding. |
| **Port forwarding** | Regra no roteador que encaminha uma porta externa para um dispositivo da rede interna. |
| **Sala por chave (room key)** | Chave secreta que dá acesso a uma sala; separa hosts e podcasts diferentes. |
| **Stream key** | Chave fornecida pela plataforma (Twitch, YouTube, Kick) para publicar uma transmissão. |
| **Keyring** | Biblioteca Rust que guarda segredos no keychain do sistema operacional. |
| **Ingest** | Endpoint para onde o encoder envia o stream de vídeo. |
| **RTMP / RTMPS** | Protocolo de ingest de vídeo; RTMPS é a versão com TLS. |
| **Encode 1×** | Codificar o vídeo uma única vez e reaproveitar o resultado para todas as plataformas. |
| **Fan-out** | Distribuição de um mesmo stream para vários destinos. |
| **`-c copy`** | Opção do FFmpeg que copia os streams sem recodificar. |
| **`tee`** | Muxer do FFmpeg que envia uma saída para vários destinos; todas dependem do mesmo processo. |
| **Overlay** | Camada visual sobre o vídeo, como logo ou texto. Renderizada pelo compositor nativo. |
| **Diretor** | Máquina que monta a cena e transmite o programa. |
| **Hot failover** | Redundância em que o standby já está pronto para assumir a publicação. |
| **i18n** | Internacionalização; o app atende PT-BR e EN. |

# Relationships

- [Stack](/componentes/stack.md) — tecnologias citadas aqui.
- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — termos de captura, composição, encode e fan-out em uso.
