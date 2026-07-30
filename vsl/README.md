# Clone da página (dailywellnessgood.com/v91)

## O que foi feito

- HTML clonado a partir da página original, com as 34 imagens baixadas para `assets/images/`.
- Removidos todos os parâmetros de rastreio/afiliação da URL de origem (`cid`, `utm_*`, `sck`, `rtkcid`, `rtkcmpid`).
- Removidos os seguintes scripts/trackers que identificavam ou beneficiavam o dono original:
  - Microsoft Clarity (analytics de sessão)
  - Pixel do Taboola (`_tfa`)
  - Script `go.dailywellnessgood.com/track.js` (loader do RedTrack)
  - Script de postback do RedTrack que disparava evento "InitiateCheckout" para `eo5uy.ttrk.io`
  - Script ofuscado (`atob(...)`) que carregava `cdn.albumcompleto.org` — código escondido de "cloaking"/heartbeat, também removido
  - Script de heartbeat (`/hb?...`) que reportava presença do visitante a cada 30s
  - Script de sequestro do botão "voltar" do navegador (redirecionava para `.../preclick`)
  - Script que propagava os parâmetros de rastreio para todos os links da página
  - Script `rtk-cookie-fallback` que injetava `clickid` nos links de compra
  - Preloads/DNS-prefetch que apontavam para a conta ConverteAI/VTurb do dono original
- Os 3 botões "Buy Now" tinham `href="https://go.dailywellnessgood.com/click/1|2|3"` (link de redirecionamento/tracking do dono). Foram trocados por `href="#SUBSTITUA-PELO-LINK-DE-CHECKOUT"` — troque pelo seu link de checkout/oferta.
- O e-mail de contato (ofuscado via Cloudflare) foi trocado pelo placeholder `suporte@seusite.com`.
- **Mantido de propósito**: o script que revela os botões de compra (classe `.esconder`) depois de um tempo assistido de vídeo ou por um timer de segurança — isso é lógica de funcionamento da página, não rastreio do dono.

## Onde colocar sua VSL

Procure por `id="vsl-placeholder"` em `index.html` (dentro da seção `<div class="video_vturb" id="video">`, por volta da linha ~4285). Há um bloco bem marcado assim:

```html
<!-- ============================================================ -->
<!-- COLOQUE SUA VSL AQUI                                          -->
<!-- Substitua este bloco pelo embed do seu player de vídeo        -->
<!-- (VTurb, Panda Video, YouTube, Vimeo, HTML5 <video>, etc.)     -->
<!-- ============================================================ -->
<div id="vsl-placeholder" ...>
    INSIRA SUA VSL AQUI
</div>
```

Basta apagar essa `div` e colar o embed do seu player no lugar.

### Atenção ao script de "revelar botão de compra"

Mais abaixo no HTML existe um script que espera **45min10s** de vídeo assistido (via `smartplayer.instances[0]`, API específica do player VTurb) para revelar os botões de compra escondidos (classe `.esconder`). Se você:

- **Usar o VTurb**: funciona automaticamente, sem alterar nada.
- **Usar outro player**: adapte o evento `timeupdate` do script para o seu player, ou simplesmente confie no fallback por tempo (`setTimeout`) que já existe na página e funciona independente do vídeo.

## Estrutura de arquivos

```
dailywellness-clone/
├── index.html                          ← página clonada e limpa (use este arquivo)
├── assets/images/                      ← 34 imagens baixadas do site original
└── _original-referencia-NAO-USAR.html  ← cópia bruta original, só para conferência/comparação
```

## Como testar localmente

Abra um terminal na pasta e rode:

```
python -m http.server 8000
```

Depois acesse `http://localhost:8000/index.html` no navegador.

## Observação

O arquivo `_original-referencia-NAO-USAR.html` é a cópia bruta da página original (com todos os trackers) — mantida apenas como referência de comparação. Não publique esse arquivo.
