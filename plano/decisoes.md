---
type: Decision Register
title: "Decisões"
description: "Registro das decisões do planejamento: app nativo em Rust, Windows e Linux, sem Electron e sem webview, com LiveKit para convidados."
tags: [decisoes, registro, rust]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Decisões tomadas

| # | Tema | Decisão | Motivo |
|---|---|---|---|
| D1 | Linguagem e shell | **Rust puro**; sem Electron e sem webview | Requisitos do projeto: Rust obrigatório, Electron vetado, e webview não cobre a captura no Linux. |
| D2 | Interface | **UI 100% Rust** (Slint ou egui/iced; escolha específica em aberto) | Coerente com "sem webview"; roda em Windows e Linux. |
| D3 | Plataformas do MVP | **Windows + Linux** | Requisito do MVP. |
| D4 | Transporte de mídia | **LiveKit** (SFU) + TURN; convidados via **SDK Rust do LiveKit**; app flexível para relay externo | Salas, tokens e reconexão prontos; convidados chegam pela internet. |
| D5 | Servidor de mídia no MVP | **LiveKit local** (Docker por enquanto; alvo: binário `livekit-server` empacotado) | Escolha do responsável, com migração prevista. |
| D6 | Onde compõe e encoda | **Na máquina do diretor** (local-first), núcleo nativo Rust; sem servidor dedicado | O host é o próprio servidor. |
| D7 | Captura e composição | **Nativas em Rust** nas duas plataformas; composição em `wgpu` | Webview não captura tela no Linux; requisito de app nativo. |
| D8 | Redundância | **Ativo/standby (hot failover)** entre os dois hosts | A live não pode cair; as plataformas aceitam um único publicador por stream key. |
| D9 | Acesso do convidado | Instala o app e entra com a **chave de sala** | Isolamento entre hosts e entre podcasts diferentes. |
| D10 | Alcance da rede | **Internet desde o início** | Podcast remoto, sem local físico. |
| D11 | Escopo do MVP | **1 convidado remoto**, em Windows e Linux | Escolha do responsável pelo produto. |
| D12 | Idioma | **PT-BR e EN** (i18n em Rust) | Escolha do responsável pelo produto. |

> **Nota sobre D2:** Slint é a recomendação para cara de produto; egui é boa para painel/dashboard. A escolha fica para o início da [Fase 1](/roadmap/fase-1-mvp.md).
> **Nota sobre D5:** o alvo é o `livekit-server` empacotado como processo filho (binário, cross-platform). A migração do Docker para esse formato deve ser definida na [Fase 1](/roadmap/fase-1-mvp.md).

# Alternativas descartadas

- **Electron**: Chromium embutido; vetado pelo requisito.
- **Tauri / webview**: o webview do Linux (WebKitGTK) não faz captura de tela de forma confiável.
- **Captura/composição no webview** (`getDisplayMedia` + WebCodecs) e **Track Egress**: dependem de Chromium; substituídos por captura e composição nativas.
- **Construir sobre libobs (C++)**: mais lento e fora do requisito de Rust.
- **mediasoup puro** (em relação a D4): mais controle, mas mais trabalho.
- **Janus / Jitsi** (em relação a D4): ecossistema de composição e egress menos direto.
- **P2P mesh**: não escala e falha atrás de NAT sem TURN.
- **MCU / mix na nuvem**: servidor caro e maior latência.
- **Composição no servidor (egress)**: exige servidor com GPU e custo maior.
- **`tee` para as três plataformas**: as saídas dependem do mesmo processo, então uma falha pode derrubar as outras.

# Relationships

- [Modelo consolidado](/arquitetura/modelo-consolidado.md) — consolida D6, D7 e D8.
- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — implementa D7.
- [Stack](/componentes/stack.md) — implementa D1, D2 e D12.
- [Infraestrutura de transporte](/arquitetura/infraestrutura-de-transporte.md) — contexto de D4 e D10.
- [Redundância ativo/standby](/arquitetura/redundancia-ativo-standby.md) — detalha D8.
- [Salas por chave](/arquitetura/salas-por-chave.md) — detalha D9.
