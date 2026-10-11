# Plano — OBS Próprio (Multistream + Convidados Remotos)

Estúdio de transmissão instalável, com **multistream nativo** (Twitch, YouTube e Kick), **convidados remotos via WebRTC** (estilo Discord) e **overlays**, inspirado no OBS Studio.

Este bundle substitui o antigo `PLANO.md` monolítico. Status do bundle: **rascunho**, aguardando revisão. Comece pela [visão geral](/visao-geral.md). Cada pasta tem seu próprio `index.md`.

## Conceitos de referência

* [Visão geral](/visao-geral.md) - Objetivo, capacidades centrais, contexto e resultado do MVP.
* [Glossário](/glossario.md) - Termos técnicos usados no plano, como SFU, TURN, CGNAT e fan-out.
* [Decisões](/decisoes.md) - Decisões já tomadas, com motivo e alternativas descartadas.
* [Riscos](/riscos.md) - Riscos identificados e suas mitigações.

## Seções

* [Arquitetura](/arquitetura/index.md) - Modelo consolidado, pipeline de mídia, salas por chave, redundância e transporte.
* [Componentes](/componentes/index.md) - Stack, sidecars (LiveKit, MediaMTX, FFmpeg) e endpoints de ingest.
* [Roadmap](/roadmap/index.md) - Fases 0 a 4, do spike técnico à escala.
* [Pendências](/pendencias/index.md) - Spikes e decisões ainda em aberto.

## Histórico

* [Log de alterações](/log.md) - Registro cronológico de mudanças no bundle.
