---
type: Open Question
title: "Estrutura do repositório"
description: "Definir a organização do workspace Rust (crates do núcleo de mídia, UI e integrações) e as convenções de código."
tags: [pendencia, repositorio, rust, workspace]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Pergunta

Como organizar o workspace Rust (crates separadas para UI, núcleo de mídia, captura, composição, encode e WebRTC) e quais convenções de código adotar?

# Contexto

O workspace ainda não contém código da aplicação. Hoje ele tem apenas este bundle e a skill OKF em `.agents/`. É um app nativo em Rust, não um workspace Tauri.

# Relationships

- [Fase 1 — MVP](/roadmap/fase-1-mvp.md) — primeira fase que exige código.
- [Stack](/componentes/stack.md) — tecnologias do projeto.
