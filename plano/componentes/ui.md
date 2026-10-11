---
type: Component
title: "UI"
description: "Interface 100% Rust, sem webview, construída em Slint ou egui/iced para Windows e Linux."
tags: [componente, ui, rust, slint, egui]
sources:
  - id: sessao-planejamento
    resource: "Sessão de planejamento com o responsável pelo produto (2026-10)"
generated:
  by: agent:opencode/claude-haiku-5-5
  at: 2026-10-11T00:00:00Z
status: draft
---

# Papel

A interface é **100% Rust**, sem webview e sem JavaScript.

| Toolkit | Perfil indicado |
|---|---|
| **Slint** | Cara de produto; linguagem declarativa própria; recomendado. |
| **egui** | Immediate mode; bom para painel/dashboard. |
| **iced** | Retained mode; estrutura em estilo Elm. |

# Conteúdo do painel

- Seleção de cenas e fontes.
- Configuração de destinos (Twitch, YouTube, Kick).
- Status e bitrate por plataforma; botão "Ao vivo".
- Gestão da sala por chave (compartilhar a chave com os convidados).

# Consequências

- Como não há HTML/CSS, os **overlays são renderizados no núcleo** ([Captura e composição](/componentes/captura-e-composicao.md)), não na interface.
- i18n em Rust: PT-BR e EN.

# Relationships

- [Stack](/componentes/stack.md) — escolhas de tecnologia.
- [Decisões](/decisoes.md) — D2 (UI em Rust).
