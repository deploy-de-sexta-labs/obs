# Plano — OBS Próprio (Multistream + Convidados Remotos)

> Estúdio de transmissão instalável, com **multistream nativo** (Twitch + YouTube + Kick),
> **convidados remotos via WebRTC** (estilo Discord) e **overlays**.
> Inspirado no OBS Studio, mas com os diferenciais pensados desde o início.

---

## 1. Visão e objetivo

Construir um aplicativo desktop instalável que substitua o OBS Studio para o fluxo do
podcast **"Deploy de Sexta"**, com três capacidades centrais:

1. **Multistream nativo** — 1 captura → Twitch + YouTube + Kick simultaneamente.
2. **Convidados remotos** — participantes entram por uma sala WebRTC (estilo Discord),
   remotamente pela internet, com isolamento por *chave de sala*.
3. **Overlays** — camadas visuais (logo, texto, etc.) sobre o vídeo.

Contexto:
- O podcast é **remoto** (sem local físico); todos os participantes estão na internet.
- **Sem servidor dedicado**: o SFU roda na máquina de um dos hosts, junto com o app.
- Dois hosts podem atuar como **diretor** para **redundância** (a live não cair).

**Resultado da primeira fase (MVP):** um fluxo funcional com **1 host + 1 convidado remoto**
transmitindo ao vivo para as 3 plataformas, com overlay e reconexão independente por plataforma.

---

## 2. Modelo consolidado

- **App instalável** (Tauri) — não é via navegador. Mesmo app serve para **host/diretor** e
  para o **modo convidado**.
- **Sem servidor dedicado.** O `livekit-server` (SFU) roda na máquina de um dos hosts,
  gerenciado como *sidecar* pelo app.
- **Salas por chave (multi-tenant).** O host possui um *room key*; só quem tem a chave entra
  na sala. Chaves diferentes = salas diferentes = podcasts diferentes, **sem cruzamento de dados**.
  O próprio host (que detém o segredo do SFU) emite o token de acesso escopado àquela sala.
- **Redundância ativo/standby** entre os dois hosts para a saída das plataformas.
- **Convidados são remotos** e **não publicam para as plataformas** — entram apenas na composição.

### Papéis

| Papel | Quem é | Responsabilidade |
|---|---|---|
| **Participante** | cada host e cada convidado | captura a própria câmera+mic e envia **um único** stream |
| **Transporte (SFU)** | `livekit-server` na máquina de um host | encaminha mídia (sem recomprimir), resolve NAT, reconexão |
| **Diretor / Programa** | máquina que transmite | baixa todos, **monta a cena**, mixa áudio, **encoda 1×** e faz fan-out para as 3 plataformas |

---

## 3. Stack

| Camada | Escolha | Por quê |
|---|---|---|
| Shell | **Tauri v2** (Rust) | app leve; backend Rust ideal para orquestrar binários sidecar |
| Frontend | **React + TypeScript + Tailwind** | reusa conhecimento do projeto; overlays em HTML/CSS |
| Webview | **WebView2** (Windows) | Chromium: WebRTC, `getDisplayMedia`, WebCodecs |
| Sidecars (geridos pelo Rust) | `ffmpeg`, `mediamtx`, `livekit-server` | encode/fan-out, relay local, SFU |
| SDK de mídia | `livekit-client` (JS no webview) | publicar/assinar tracks, salas, reconexão |
| Empacotamento | Tauri bundler (instalador Windows primeiro) | `.msi`/`.exe` |
| i18n | **i18next** | PT-BR + EN |
| Segredos | **keychain do SO** | stream keys nunca em texto puro |

> Windows é o alvo primário. macOS/Linux ficam para depois (WebView usa WebKit, o que pode
> exigir fallbacks de captura).

---

## 4. Arquitetura de mídia

Como há convidados desde o MVP, os tracks WebRTC chegam no webview; portanto a **composição
acontece no webview** (canvas + overlays HTML/CSS). Fluxo:

```
WebView2 (canvas): host cam + guest cam + overlays + mix de áudio
        │  (a cena composta é publicada como 1 track WebRTC na sala LiveKit local)
        ▼
livekit-server (sidecar)  ──►  saída do programa
        ▼
encode 1× (GPU: NVENC/QSV/AMF, fallback libx264)
        ▼
MediaMTX local (rtmp://localhost/live)
        ├─ FFmpeg copy ─► Twitch    (reconnect próprio)
        ├─ FFmpeg copy ─► YouTube   (reconnect próprio)
        └─ FFmpeg copy ─► Kick      (RTMPS, reconnect próprio)
```

**Por que relay + fan-out com `-c copy`:**
- **Encode único** (barato, em GPU).
- **Cada plataforma independente**: uma cair não derruba as outras (≠ `tee`, que compartilha o destino).
- O relay local é também o **ponto natural** onde o host *standby* assume no failover.

### Ingest endpoints conhecidos

| Plataforma | Endpoint |
|---|---|
| Twitch | `rtmp://live.twitch.tv/app/<stream_key>` |
| YouTube | `rtmp://a.rtmp.youtube.com/live2/<stream_key>` |
| Kick | `rtmps://fa723fc1b171.global-contribute.live-video.net/app/<stream_key>` |

### Spike pendente (Fase 0/1): webview → FFmpeg

Como levar a cena composta do webview até o FFmpeg. Duas vias candidatas; testar ambas e escolher:

1. **LiveKit Track Egress** (sidecar) pega o track do programa e gera RTMP → MediaMTX.
2. **WebCodecs** encoda (HW) no webview → Rust → FFmpeg apenas muxa (`-c copy`).

A via escolhida fica **atrás de uma interface**, para troca barata depois.

---

## 5. Salas por chave (isolamento multi-tenant)

- Host abre sessão → o app gera/usa um **room key**.
- O `livekit-server` local emite **tokens JWT escopados à sala** (só quem tem a chave recebe token).
- **Chave = sala = isolamento total.** Outros podcasts usam o mesmo app, cada um com seu SFU/sala.
- O segredo do `livekit-server` fica somente na máquina do host.

---

## 6. Redundância (ativo/standby)

Design recomendado para minimizar pontos de falha sem servidor dedicado:

- **Diretor ATIVO** roda em um host e publica para as plataformas.
- **SFU roda no host STANDBY**, e o **Diretor STANDBY** assina a mesma cena (determinística), já "quente".
- Se o **diretor ativo** cai: o standby assume a publicação em segundos; os convidados **não caem** (SFU vivo).

Limitação explícita: se cair o **host que roda o SFU**, tudo cai. Cobrir isso exige um
3º elemento sempre-on — por isso o app é **flexível para relay externo**.

### Restrição de plataformas

Twitch/YouTube/Kick aceitam **um único publicador** por stream key. Logo "os dois diretores
simultâneos" só funciona como **failover ativo/standby** (não como publicação dupla).

---

## 7. Infraestrutura de transporte (ponto de atenção)

"Sem servidor dedicado" + "convidados pela internet" se **chocam no nível de rede**.
WebRTC precisa de endereço público. Opções:

- **Port forwarding no roteador do host** — sem custo; **falha sob CGNAT** (comum hoje). Testar primeiro.
- **Relay TURN público** — necessário atrás de NAT simétrico; também exige IP público.
- **VPS mínimo (US$5/mês)** apenas para TURN **ou** SFU — composição e encode continuam locais.

**Decisão adotada:** app **flexível** — funciona com SFU/TURN local **ou** apontando para um
relay externo (VPS mínimo ou gerenciado), mantendo composição e encode na máquina local.

---

## 8. Roadmap

### Fase 0 — Spike (1–2 dias)
- Script FFmpeg: captura tela + câmera + áudio → 3 plataformas.
- Validar ingests (incl. **Kick RTMPS**).
- **Medir upload** (multistream ≈ 3× o bitrate) — confirmar que a internet aguenta.
- Decidir a via de saída: **Track Egress** vs **WebCodecs**.

### Fase 1 — MVP (1 convidado remoto)
- App Tauri instalável (Windows).
- Host: câmera + mic + tela; **1 convidado** via LiveKit (sidecar + TURN); sala por chave.
- Cena **host + convidado**; overlay de logo/texto; mixer de áudio básico.
- Multistream 3× via MediaMTX + FFmpeg copy, com **reconexão por plataforma**.
- Painel com status/bitrate por plataforma; botão "Ao vivo".
- i18n PT-BR + EN. Stream keys no keychain.

### Fase 2 — Redundância
- Eleição de líder + **failover ativo/standby** entre os dois hosts.
- SFU no standby, cena determinística replicada.

### Fase 3 — Recursos OBS-like
- Múltiplas cenas, transições, gravação local, mixer por fonte.

### Fase 4 — Escala e polimento
- N convidados, overlays avançados (browser source), perfis de transmissão.

---

## 9. Riscos e mitigações

| # | Risco | Mitigação |
|---|---|---|
| 1 | CGNAT / alcance dos convidados | app flexível (SFU/TURN local ou relay externo) |
| 2 | Levar cena do webview ao FFmpeg | spike das 2 vias (Track Egress / WebCodecs), atrás de interface |
| 3 | Banda de upload 3× (≈18 Mbps p/ 6 Mbps) | medir na Fase 0 |
| 4 | `getDisplayMedia`/WebCodecs no WebView2 | testar cedo; fallback captura nativa via Rust |
| 5 | Segurança das stream keys | armazenar no keychain do SO |

---

## 10. Decisões já tomadas

- [x] Stack: **Tauri** (não Electron), React + TS + Tailwind, i18next.
- [x] Transporte: **LiveKit** (sidecar) + TURN; app **flexível** para relay externo.
- [x] Redundância: **ativo/standby (hot failover)**.
- [x] Convidado: instala o app e entra com a **chave de sala**.
- [x] Alcance: **internet desde o início** (podcast remoto).
- [x] Escopo do MVP: **já com 1 convidado remoto**.

## 11. Decisões em aberto / próximos spikes

- [ ] Via de saída do programa: **Track Egress** vs **WebCodecs** (Fase 0/1).
- [ ] Alvo de resolução/FPS e bitrate por plataforma (ex.: 1080p30, ~6 Mbps).
- [ ] Estrutura de repositório e convenções de código.
