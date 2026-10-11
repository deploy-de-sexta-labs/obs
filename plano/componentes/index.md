# Componentes

Peças do sistema: stack, interface, núcleo nativo de mídia, binários de apoio e endpoints externos.

* [Stack](/componentes/stack.md) - Tecnologias por camada, em Rust, para Windows e Linux.
* [UI](/componentes/ui.md) - Interface 100% Rust, sem webview.
* [Captura e composição](/componentes/captura-e-composicao.md) - Captura nativa por sistema operacional e compositor em wgpu.
* [LiveKit server](/componentes/livekit-server.md) - SFU que transporta a mídia dos participantes.
* [MediaMTX](/componentes/mediamtx.md) - Relay RTMP local usado no fan-out.
* [FFmpeg](/componentes/ffmpeg.md) - Encode e envio para as plataformas.
* [Ingests das plataformas](/componentes/ingests-das-plataformas.md) - Endpoints de publicação de Twitch, YouTube e Kick.
