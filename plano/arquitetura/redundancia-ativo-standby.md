---
type: Architecture
title: "Redundância ativo/standby"
description: "Failover entre os dois hosts: um diretor publica, o outro fica quente e assume se o ativo cair, com o SFU no standby."
tags: [arquitetura, redundancia, failover, standby]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Desenho

- **Diretor ATIVO** roda em um host e publica para as plataformas.
- **SFU roda no host STANDBY**, e o **Diretor STANDBY** assina a mesma cena (determinística), já "quente".
- Se o **diretor ativo** cai, o standby assume a publicação em segundos, e os convidados **não caem**, porque o SFU continua no ar.

# Limitação

Se cair o **host que roda o SFU**, tudo cai. Cobrir esse caso exige um terceiro elemento sempre ligado. Por isso, o app é **flexível para relay externo** (ver [Infraestrutura de transporte](/arquitetura/infraestrutura-de-transporte.md)).

# Restrição das plataformas

Twitch, YouTube e Kick aceitam **um único publicador** por stream key. Por isso, "os dois diretores simultâneos" só funciona como **failover ativo/standby**, e não como publicação dupla.

# Relationships

- [Fase 2 — Redundância](/roadmap/fase-2-redundancia.md) — implementação do failover.
- [Ingests das plataformas](/componentes/ingests-das-plataformas.md) — restrição de publicador único.
- [Decisões](/decisoes.md) — D8.
- [Pipeline de mídia](/arquitetura/pipeline-de-midia.md) — relay local como ponto de troca.
