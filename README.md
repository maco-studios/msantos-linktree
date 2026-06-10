![M. Santos](./public/profile.png)

[![Vue](https://img.shields.io/badge/Vue-3-42b883.svg)](https://vuejs.org/)
[![Vite](https://img.shields.io/badge/Vite-8-646cff.svg)](https://vite.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-6-3178c6.svg)](https://www.typescriptlang.org/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3-38bdf8.svg)](https://tailwindcss.com/)
[![GitHub Pages](https://img.shields.io/badge/Deploy-GitHub_Pages-222.svg)](https://pages.github.com/)

# M. Santos Linktree

Linktree estatico para a **M. Santos - Mecanica Automotiva**, feito para uso na bio do Instagram.

O projeto concentra as principais acoes comerciais da oficina em uma pagina leve, responsiva e publicada no GitHub Pages: WhatsApp, ligacao, rota no Google Maps, avaliacao no Google, horario de atendimento dinamico e mapa incorporado.

## Live Demo

```txt
https://maco-studios.github.io/msantos-linktree/
```

## Features

- Layout responsivo e compacto para acesso pelo Instagram.
- CTA principal para agendamento via WhatsApp com mensagem pre-preenchida.
- Botao para ligar diretamente para a oficina.
- Google Maps incorporado e botao de rota.
- CTA para avaliacao da empresa no Google.
- Status de atendimento dinamico conforme horario comercial.
- Eventos do Google Analytics por botao.
- SEO com meta tags, Open Graph, Twitter Card, JSON-LD, sitemap e robots.txt.
- Deploy automatico no GitHub Pages via GitHub Actions.
- Fontes locais: Hudson NY para titulos e Montserrat para textos.
- Icones com Heroicons e Font Awesome Brands.
- Animacoes sutis e textura visual no fundo e nos elementos.

## Tech Stack

- Vue 3
- TypeScript
- Vite
- Tailwind CSS
- Heroicons
- Font Awesome
- Google Analytics
- GitHub Pages

## Installation

Clone o projeto e instale as dependencias:

```bash
git clone git@github.com:maco-studios/msantos-linktree.git
cd msantos-linktree
npm install
```

## Development

Inicie o servidor local:

```bash
npm run dev
```

O Vite exibira a URL local no terminal.

## Build

Gere a versao estatica para producao:

```bash
npm run build
```

O resultado fica em:

```txt
dist/
```

## Preview

Teste o build de producao localmente:

```bash
npm run preview
```

## GitHub Pages

O projeto usa um prefixo de URL porque e publicado em um subdiretorio do GitHub Pages:

```txt
/msantos-linktree/
```

A configuracao fica em:

```ts
// vite.config.ts
base: process.env.VITE_BASE_PATH ?? '/msantos-linktree/'
```

O workflow em `.github/workflows/deploy.yml` publica automaticamente o conteudo de `dist` quando houver push na branch `main`.

No GitHub, configure:

```txt
Settings > Pages > Build and deployment > Source > GitHub Actions
```

## Environment

Para publicar em outro caminho, sobrescreva `VITE_BASE_PATH`:

```bash
VITE_BASE_PATH=/outro-prefixo/ npm run build
```

Para publicar em dominio raiz:

```bash
VITE_BASE_PATH=/ npm run build
```

## Google Analytics

O projeto usa Google Analytics com o ID:

```txt
G-QRGL30M8PZ
```

Eventos disparados pelos botoes:

| Evento | Acao |
| :-- | :-- |
| `click_whatsapp_agendamento` | Clique no CTA principal de WhatsApp |
| `click_ligar_agora` | Clique no botao de telefone |
| `click_avaliar_google` | Clique no botao de avaliacao no Google |
| `click_como_chegar` | Clique no botao de rota |

Todos os eventos usam:

```ts
event_category: 'linktree_button'
event_label: '<nome do botao>'
```

## Customization

As principais informacoes ficam em:

```txt
src/App.vue
```

Edite nesse arquivo:

- telefone da empresa;
- numero do WhatsApp;
- link de avaliacao no Google;
- URL do Google Maps;
- texto da mensagem pre-preenchida do WhatsApp;
- servicos exibidos;
- horario comercial;
- textos do perfil.

Metadados de SEO ficam em:

```txt
index.html
public/robots.txt
public/sitemap.xml
```

## Project Structure

```txt
.
├── .github/workflows/deploy.yml
├── public/
│   ├── favicon.svg
│   ├── profile.png
│   ├── robots.txt
│   └── sitemap.xml
├── src/
│   ├── assets/
│   ├── App.vue
│   ├── main.ts
│   └── style.css
├── index.html
├── package.json
├── tailwind.config.js
└── vite.config.ts
```

## FAQ

#### Por que o site usa `/msantos-linktree/` no caminho?

Porque o GitHub Pages publica repositorios de projeto em um subdiretorio no formato `https://usuario.github.io/repositorio/`.

#### Por que o `index.html` local ainda aponta para `/src/main.ts`?

Esse e o comportamento normal do Vite em desenvolvimento. No build de producao, o Vite substitui esse caminho por arquivos versionados em `dist/assets`.

#### Como evitar erro 404 em `/src/main.ts` no GitHub Pages?

Configure o GitHub Pages para usar **GitHub Actions**, nao deploy legado a partir da branch. O workflow publica o build correto de `dist`.

#### Onde troco o link do Google Maps?

Em `src/App.vue`, nas constantes `mapsUrl` e `mapEmbedUrl`.

#### Onde troco o ID do Google Analytics?

Em `index.html`, no snippet do Google tag.

## Acknowledgements

- [Vue](https://vuejs.org/)
- [Vite](https://vite.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Heroicons](https://heroicons.com/)
- [Font Awesome](https://fontawesome.com/)
- [Awesome README](https://github.com/matiassingers/awesome-readme)
