# Clone de aureviasam.com/vsl1-aarp-neuromind-willis

Clone estático (HTML/CSS/JS puro) da página de VSL, com todo o rastreio removido e um espaço marcado para você colocar sua própria VSL.

## Estrutura

```
aureviasam-clone/
├── index.html      ← página clonada e limpa
├── css/main.css    ← estilos (Elementor) baixados do site original
├── js/             ← scripts (jQuery, Elementor, etc.) baixados do site original
└── images/         ← todas as imagens/ícones baixados (potes do produto, prints de "comentários", etc.)
```

Basta abrir `index.html` num navegador (ou subir a pasta inteira num host qualquer) para ver a página funcionando com os caminhos locais.

## O que foi removido (rastreio)

1. **`https://rt.memorion-life.site/track.js`** — script de tracking estilo RedTrack, o mesmo que recebia os parâmetros `clickid` / `rtkcid` / `rtkcmpid` da URL que você mandou.
2. Um script de **"auto-clique"** que simulava um clique no meio da tela no primeiro toque/clique do visitante — estava amarrado ao mesmo domínio de rastreio acima (dark pattern, não é rastreio em si, mas foi removido junto).
3. Os 3 botões **"ADD TO CART"** apontavam para `https://track.aureviasam.com/click/1|2|3` (links de redirecionamento/afiliado). Foram trocados por âncoras `#COLOQUE-AQUI-O-LINK-DE-CHECKOUT-...` — troque pelo seu link de checkout/afiliado real.
4. No `<head>`: canonical, `og:url`, JSON-LD (schema.org) e feeds RSS/oEmbed/wp-json que apontavam para o domínio de origem do funil (`healthoptimizer.online`), o `dns-prefetch` para `googletagmanager.com`, e o `<meta name="generator" content="Site Kit by Google">` (plugin de Analytics/Search Console).
5. Endpoints internos do Elementor (`admin-ajax.php`, `wp-json`, URLs de assets) que ainda apontavam para o site original foram neutralizados — eles só serviriam para recursos que esta página não usa (formulários/popups do Elementor Pro).

Nada de Meta Pixel, Google Ads/Analytics (gtag/GTM), TikTok Pixel ou similares foi encontrado no código-fonte — o único rastreamento ativo era o script RedTrack acima.

## Onde colocar sua VSL

Procure por este bloco em `index.html` (é a área logo abaixo do cabeçalho, onde ficava o vídeo):

```html
<!-- ======================================================================
     >>> COLOQUE AQUI SUA VSL (vídeo de vendas) <<<
     ...
     ====================================================================== -->
<div class="vslx">
  <!-- Exemplo: <video controls poster="images/poster-vsl.jpg" ...><source src="SEU-VIDEO.mp4" type="video/mp4"></video> -->
  <!-- Exemplo: cole aqui o <script> do seu player VTurb/Converteai -->
</div>
```

O player original (VTurb/Converteai, de terceiros — não é seu) foi removido, junto com os `<link rel="preload">` e `dns-prefetch` que ele usava. Mantenha a `<div class="vslx">` para preservar o CSS de tamanho/posição já existente no `<head>` (regra `#fix-vsl-box`, que limita a largura a 400px no mobile).

## Links de checkout

Os 3 botões "ADD TO CART!" (planos de 2, 3 e 6 potes) estão com `href="#COLOQUE-AQUI-O-LINK-DE-CHECKOUT-..."`. Troque cada um pelo link real de pagamento/checkout/afiliado antes de publicar.
