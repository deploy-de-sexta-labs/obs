---
type: Glossary
title: "Glossário"
description: "Termos de transporte, mídia e streaming usados no plano."
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
| **Sidecar** | Binário externo (ffmpeg, mediamtx, livekit-server) iniciado e gerido pelo app Tauri. |
| **SFU** (Selective Forwarding Unit) | Servidor que recebe o stream de cada participante e o encaminha aos demais sem recomprimir. |
| **WebRTC** | Tecnologia de comunicação em tempo real (áudio, vídeo e dados) usada no navegador e no webview. |
| **STUN** | Servidor que ajuda um participante a descobrir seu endereço público; não retransmite mídia. |
| **TURN** | Servidor que retransmite a mídia quando a conexão direta não é possível. |
| **NAT simétrico** | Tipo de NAT que costuma impedir a conexão direta entre pares, exigindo TURN. |
| **CGNAT** | Carrier-Grade NAT: o provedor compartilha um IP público entre vários clientes, impedindo port forwarding. |
| **Port forwarding** | Regra no roteador que encaminha uma porta externa para um dispositivo da rede interna. |
| **Sala por chave (room key)** | Chave secreta que dá acesso a uma sala; separa hosts e podcasts diferentes. |
| **Stream key** | Chave fornecida pela plataforma (Twitch, YouTube, Kick) para publicar uma transmissão. |
| **Keychain do SO** | Cofre de credenciais do sistema operacional, onde as stream keys ficam guardadas. |
| **Ingest** | Endpoint para onde o encoder envia o stream de vídeo. |
| **RTMP / RTMPS** | Protocolo de ingest de vídeo; RTMPS é a versão com TLS. |
| **Encode 1×** | Codificar o vídeo uma única vez e reaproveitar o resultado para todas as plataformas. |
| **Fan-out** | Distribuição de um mesmo stream para vários destinos. |
| **`-c copy`** | Opção do FFmpeg que copia os streams sem recodificar. |
| **`tee`** | Muxer do FFmpeg que envia uma saída para vários destinos; todas dependem do mesmo processo. |
| **NVENC / QSV / AMF** | Codificadores de hardware de NVIDIA, Intel e AMD, respectivamente. |
| **libx264** | Codificador H.264 por software, usado como fallback. |
| **Track** | Fluxo individual de áudio ou vídeo dentro de uma conexão WebRTC. |
| **Track Egress** | Serviço do LiveKit que exporta um track publicado (no plano, para RTMP). |
| **WebCodecs** | API do navegador para codificar e decodificar vídeo e áudio, com aceleração de hardware quando disponível. |
| **Composição** | Montagem da cena final: layout, overlays e mix de áudio. |
| **Overlay** | Camada visual sobre o vídeo, como logo ou texto. |
| **Diretor** | Máquina que monta a cena e transmite o programa. |
| **Hot failover** | Redundância em que o standby já está pronto para assumir a publicação. |
| **WebView2** | Componente da Microsoft baseado em Chromium, usado pelo Tauri no Windows. |
| **Tauri** | Framework de apps desktop com backend em Rust e interface em webview. |

# Relationships

- [Stack](/componentes/stack.md) — tecnologias citadas aqui.
- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — termos de encode e fan-out em uso.
