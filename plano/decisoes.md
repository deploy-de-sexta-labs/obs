---
type: Decision Register
title: "Decisões"
description: "Registro das decisões já tomadas no planejamento, com motivo e alternativas descartadas."
tags: [decisoes, registro]
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
| D1 | Shell do app | Tauri v2, não Electron | Software mais leve; backend Rust para orquestrar os sidecars. |
| D2 | Transporte de mídia | LiveKit como SFU (sidecar) + TURN; app flexível para relay externo | Salas, tokens e reconexão prontos; convidados chegam pela internet. |
| D3 | Servidor de mídia no MVP | LiveKit local via Docker, por enquanto | Escolha do responsável para o MVP. Ver nota abaixo. |
| D4 | Onde compõe e encoda | Na máquina do diretor (local-first), sem servidor dedicado | O host é o próprio servidor. |
| D5 | Redundância | Ativo/standby (hot failover) entre os dois hosts | A live não pode cair; as plataformas aceitam um único publicador por stream key. |
| D6 | Acesso do convidado | Instala o app e entra com a chave de sala | Isolamento entre hosts e entre podcasts diferentes. |
| D7 | Alcance da rede | Internet desde o início | Podcast remoto, sem local físico. |
| D8 | Escopo do MVP | Já com 1 convidado remoto | Escolha do responsável pelo produto. |
| D9 | Idioma | PT-BR e EN desde o início, com i18next | Escolha do responsável pelo produto. |

> **Nota sobre D3:** o modelo-alvo continua sendo o `livekit-server` gerido como sidecar pelo app. A migração do Docker para sidecar não está detalhada no plano e deve ser definida até o fim da [Fase 1](/roadmap/fase-1-mvp.md).

# Alternativas descartadas

- **Electron** (em relação a D1): mais pesado que o Tauri para o objetivo do app.
- **Construir sobre libobs (C++)**: mais lento até o MVP, e convidados WebRTC não são nativos.
- **mediasoup puro** (em relação a D2): mais controle, mas mais trabalho de composição e saída.
- **Janus / Jitsi** (em relação a D2): ecossistema de composição e egress menos direto.
- **P2P mesh**: não escala além de poucos participantes e falha atrás de NAT sem TURN.
- **MCU / mix na nuvem**: servidor caro e maior latência.
- **Composição no servidor (egress)** (em relação a D4): exige servidor com GPU e custo maior.
- **`tee` para as três plataformas** (em relação ao fan-out): as saídas dependem do mesmo processo, então uma falha pode derrubar as outras.

# Relationships

- [Stack](/componentes/stack.md) — implementa D1 e D9.
- [Infraestrutura de transporte](/arquitetura/infraestrutura-de-transporte.md) — contexto de D2 e D7.
- [Redundância ativo/standby](/arquitetura/redundancia-ativo-standby.md) — detalha D5.
- [Salas por chave](/arquitetura/salas-por-chave.md) — detalha D6.
- [Modelo consolidado](/arquitetura/modelo-consolidado.md) — consolida D2, D4 e D5.
