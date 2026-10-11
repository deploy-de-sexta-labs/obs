# OBS Próprio

> Estúdio de transmissão instalável, com **multistream nativo** (Twitch, YouTube e Kick), **convidados remotos via WebRTC** (estilo Discord) e **overlays**.

Aplicativo desktop **nativo em Rust** pensado para o fluxo do podcast **Deploy de Sexta**, inspirado no OBS Studio, mas com três diferenciais tratados como capacidades de primeira classe desde o início:

1. **Multistream nativo** — uma captura, enviada simultaneamente para Twitch, YouTube e Kick.
2. **Convidados remotos** — participantes entram por uma sala WebRTC, com isolamento por chave de sala.
3. **Overlays** — camadas visuais (logo, texto e afins) sobre o vídeo.

Roda em **Windows e Linux**, sem Electron e sem webview.

## Status

**Em planejamento.** Ainda não há código da aplicação. O que existe hoje é a documentação de arquitetura e roadmap, no bundle [`plano/`](plano/index.md). Comece por lá para entender o desenho completo.

## Arquitetura (resumo)

A captura e a composição são nativas em Rust. A cena é composta em GPU (`wgpu`), encodada uma única vez e distribuída para as três plataformas com reconexão independente por destino.

```
Núcleo de mídia (Rust)
 ├ Captura: câmera, tela e áudio (nativo por sistema operacional)
 └ Convidados: tracks WebRTC via SDK Rust do LiveKit
        ▼
Composição (wgpu): layout + overlays + mix de áudio
        ▼
encode 1× (GPU: NVENC/QSV; VAAPI no Linux, AMF no Windows; fallback libx264)
        ▼
MediaMTX local (rtmp://localhost/live)
        ├─ FFmpeg copy ─► Twitch    (reconnect próprio)
        ├─ FFmpeg copy ─► YouTube   (reconnect próprio)
        └─ FFmpeg copy ─► Kick      (RTMPS, reconnect próprio)
```

- Cada plataforma aceita **um único publicador** por stream key, então a redundância entre hosts é **ativo/standby** (hot failover).
- Sem servidor dedicado: o SFU (`livekit-server`) roda na máquina de um dos hosts.
- Detalhes em [Pipeline de mídia](plano/arquitetura/pipeline-de-midia.md) e [Redundância ativo/standby](plano/arquitetura/redundancia-ativo-standby.md).

## Stack

| Camada | Escolha |
|---|---|
| Linguagem | Rust |
| UI | Slint (ou egui/iced) |
| Composição | `wgpu` + `cosmic-text`/`glyphon` |
| Captura | `xcap`/`scap`, `nokhwa`, `cpal` (+ APIs por SO) |
| Convidados | SDK Rust do LiveKit (ou `webrtc-rs`) |
| Encode e fan-out | FFmpeg + MediaMTX |
| Servidor de mídia | `livekit-server` (processo filho) |
| i18n | `fluent` (ou gettext do Slint) |
| Segredos | `keyring` |

> Ver [Stack](plano/componentes/stack.md) e [Decisões](plano/decisoes.md).

## Roadmap

| Fase | Objetivo |
|---|---|
| [Fase 0 — Spike](plano/roadmap/fase-0-spike.md) | Provar o núcleo nativo (captura em Win+Linux, encode e upload). |
| [Fase 1 — MVP](plano/roadmap/fase-1-mvp.md) | App Rust nativo, Windows e Linux, com 1 convidado remoto e multistream 3×. |
| [Fase 2 — Redundância](plano/roadmap/fase-2-redundancia.md) | Failover ativo/standby entre os dois hosts. |
| [Fase 3 — Recursos OBS-like](plano/roadmap/fase-3-recursos-obs.md) | Múltiplas cenas, transições, gravação local e mixer por fonte. |
| [Fase 4 — Escala](plano/roadmap/fase-4-escala.md) | N convidados, overlays avançados e perfis de transmissão. |

## Estrutura do repositório

```
.
├── README.md              # este arquivo
├── plano/                 # bundle OKF com o plano do projeto
│   ├── index.md           # entrada da documentação
│   ├── visao-geral.md
│   ├── glossario.md
│   ├── decisoes.md
│   ├── riscos.md
│   ├── arquitetura/       # modelo, pipeline, salas, redundância, transporte
│   ├── componentes/       # stack, UI, captura e composição, sidecars e ingests
│   ├── roadmap/           # fases 0 a 4
│   └── pendencias/        # spikes e questões em aberto
└── .agents/               # skills locais do agente
```

## Documentação

O plano é mantido como um **Knowledge Bundle** no formato Open Knowledge Format (OKF), em [`plano/`](plano/index.md). Pontos de entrada úteis:

- [Visão geral](plano/visao-geral.md)
- [Decisões](plano/decisoes.md) e [Riscos](plano/riscos.md)
- [Arquitetura](plano/arquitetura/index.md) e [Componentes](plano/componentes/index.md)
- [Pendências](plano/pendencias/index.md)

## Requisitos previstos (para desenvolvimento)

Ainda não há código, então nada disso está em uso. Quando a Fase 1 começar, espera-se precisar de:

- Toolchain do **Rust** (Windows e Linux)
- **FFmpeg** e **MediaMTX**
- **LiveKit server** (`livekit-server`)
- No Linux, servidor gráfico X11 ou Wayland (com PipeWire para captura)

## Como executar

Não disponível. O projeto está na fase de planejamento; instruções de build e execução serão adicionadas quando o código existir.

## Licença

A definir.
