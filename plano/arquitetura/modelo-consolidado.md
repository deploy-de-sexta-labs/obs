---
type: Architecture
title: "Modelo consolidado"
description: "Princípios do sistema: app nativo em Rust, Windows e Linux, sem servidor dedicado, salas por chave e redundância ativo/standby."
tags: [arquitetura, modelo, papeis, rust]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Princípios

- **App nativo em Rust**, sem Electron e sem webview. O mesmo binário serve para o **host/diretor** e para o **modo convidado**.
- Roda em **Windows e Linux** desde o MVP.
- **Sem servidor dedicado.** O `livekit-server` (SFU) roda na máquina de um dos hosts, como processo filho iniciado pelo app.
- **Salas por chave (multi-tenant).** Ver [Salas por chave](/arquitetura/salas-por-chave.md).
- **Redundância ativo/standby** entre os dois hosts para a saída das plataformas. Ver [Redundância](/arquitetura/redundancia-ativo-standby.md).
- **Convidados são remotos** e **não publicam para as plataformas**: entram apenas na composição.

# Papéis

| Papel | Quem é | Responsabilidade |
|---|---|---|
| Participante | cada host e cada convidado | captura a própria câmera e mic, e envia **um único** stream (SDK Rust do LiveKit) |
| Transporte (SFU) | `livekit-server` na máquina de um host | encaminha mídia sem recomprimir, resolve NAT e cuida da reconexão |
| Diretor / Programa | núcleo nativo em Rust na máquina que transmite | baixa todos os streams, compõe a cena (`wgpu`), mixa áudio, encoda 1× e faz fan-out para as 3 plataformas |

# Relationships

- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — como o programa chega às plataformas.
- [Captura e composição](/componentes/captura-e-composicao.md) — o núcleo nativo de mídia.
- [LiveKit server](/componentes/livekit-server.md) — implementação do papel de transporte.
- [Decisões](/decisoes.md) — D6, D7 e D8 sustentam este modelo.
