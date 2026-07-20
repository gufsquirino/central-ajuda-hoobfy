# Central de Ajuda — Hoobfy

Site estático + função serverless (assistente IA) pronto para a Vercel. Clonado da central de ajuda genérica, com identidade visual e conteúdo da Hoobfy.

## Estrutura

```
central-hoobfy/
├── index.html          → site inteiro (SPA: home, categorias, artigos, busca, chat)
├── artigos.json         → 35 artigos em 8 categorias, gerados de BASE_DE_DADOS_HOOBFY.md
├── assets/
│   ├── logo-wordmark-white.png  → logo completa (ícone + "Hoobfy"), usar em fundo escuro
│   ├── icon-white.png           → só o ícone, branco, para fundo escuro
│   └── icon-blue.png            → só o ícone, azul da marca, para fundo claro (usado no favicon)
└── api/
    └── assistente.js    → função serverless que chama a API da Anthropic
```

## Deploy na Vercel (pela interface)

1. Suba esta pasta num repositório do GitHub.
2. Na Vercel: **Add New → Project → Import** o repositório.
3. Framework Preset: **Other**. Sem build command nem output directory.
4. Em **Settings → Environment Variables**, adicione `ANTHROPIC_API_KEY` (pode reusar a mesma chave da central SNR, se quiser).
5. Deploy. Aponte o domínio em **Settings → Domains** (ex.: `ajuda.hoobfy.com`).

## Antes de ativar o widget dentro do app Hoobfy

Em `api/assistente.js`, no topo, ajuste `ORIGENS_PERMITIDAS` com o domínio real onde o app roda:

```js
const ORIGENS_PERMITIDAS = [
  "https://app.hoobfy.com",   // troque pelo domínio real do painel
];
```

Sem isso, o navegador bloqueia por CORS quando o widget (`assistente-widget.js`) rodar dentro do app.

## Identidade visual

- **Azul da marca:** `#0072EF` (extraído direto do ícone enviado).
- Header e hero em fundo escuro (`#070B14`), com a logo branca (`logo-wordmark-white.png`).
- Corpo da central em fundo claro, com o azul aparecendo em links, botões e destaques.
- Favicon: `icon-blue.png`.

Se a Hoobfy tiver um guia de marca com tom de azul ligeiramente diferente, é só trocar `--brand` no `<style>` do `index.html` — todo o resto do tema deriva dessa variável.

## Conteúdo

O `artigos.json` foi gerado a partir de `BASE_DE_DADOS_HOOBFY.md`, cobrindo: Primeiros Passos, Pagamentos, Catálogo e Produtos, HF Import, Vitrine, Checkout/Logística/Rastreio, Domínio Próprio e Publicação/SEO — do zero até a loja publicada e pronta pra vender.

**Ainda não coberto** (adicionar quando houver material): tráfego pago/campanhas, gestão de pedidos no dia a dia, automações de WhatsApp. Manda as transcrições dessas aulas quando tiver e eu expando a base do mesmo jeito.

## Assistente IA

Mesmo funcionamento da central genérica: busca os 3 artigos mais relevantes pra pergunta do usuário e manda pra `/api/assistente`, que responde só com base neles — sem inventar passo, com fallback pro suporte humano quando o assunto não estiver coberto.
