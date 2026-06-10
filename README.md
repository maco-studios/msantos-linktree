# Linktree M. Santos Auto Service

Linktree estatico para mecanica automotiva feito com Vue 3, TypeScript, Vite
e Tailwind CSS.

## Desenvolvimento

```sh
npm install
npm run dev
```

## Build

O projeto ja esta configurado para GitHub Pages com o prefixo padrao:

```sh
npm run build
```

Por padrao, o Vite usa `base: /msantos-linktree/`, necessario quando o site e publicado em:

```txt
https://maco-studios.github.io/msantos-linktree/
```

Para publicar com outro prefixo, defina `VITE_BASE_PATH`:

```sh
VITE_BASE_PATH=/meu-prefixo/ npm run build
```

Se for publicar em um dominio raiz, use:

```sh
VITE_BASE_PATH=/ npm run build
```

Edite o perfil, links, servicos, filtros e metricas em `src/App.vue`.

## GitHub Pages

O workflow em `.github/workflows/deploy.yml` publica automaticamente o conteudo de
`dist` no GitHub Pages quando houver push na branch `main`.

No GitHub, configure `Settings > Pages > Build and deployment > Source` como
`GitHub Actions`.
