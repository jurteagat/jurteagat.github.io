# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Qué es este repositorio

Blog personal en español de Juan Urteaga (https://jurteagat.github.io), construido
con **Quarto** (`project: type: website`) y **R**. No hay build system aparte de
Quarto ni tests.

## Comandos

```sh
quarto preview            # servidor local con recarga en vivo
quarto render             # genera el sitio en _site/ (gitignored)
quarto render posts/grd_2026/index.qmd   # renderizar un solo post
quarto publish gh-pages   # publica: renderiza y empuja a la rama gh-pages
```

El despliegue es manual con `quarto publish gh-pages` (GitHub Pages sirve la rama
`gh-pages`; sus commits son "Built site for gh-pages"). La rama de trabajo es
`master`.

## Arquitectura

- Cada post vive en `posts/<slug>/index.qmd` con sus recursos al lado
  (`references.bib`, `apa.csl`, imágenes, `data/`). El listado del blog es
  `posts.qmd` (listing sobre `posts/`); la portada es `index.qmd`.
- `posts/_metadata.yml` aplica a todos los posts y activa **`freeze: true`**: la
  salida computacional de los chunks R se cachea en `_freeze/` (versionado en
  git). `quarto render` NO re-ejecuta el código R de un post salvo que su `.qmd`
  cambie — por eso `_freeze/` debe commitearse cuando se edita un post.
- Los chunks son R (tidyverse, sf, leaflet, plotly, raster, spatstat, crosstalk,
  entre otros); se necesita un R con esos paquetes solo si se re-ejecuta código,
  no para renderizar desde el freeze.
- `midputs/` y `raw/` son directorios de datos locales (gitignored), usados por
  los posts vía `here::here()`; pueden no existir en un clon fresco.
- Tema HTML: flatly (claro) / darkly (oscuro) + `styles.css`, configurado en
  `_quarto.yml`.

## Particularidad del entorno

El repo vive en un disco externo **ExFAT**: macOS genera archivos basura `._*`
(ya ignorados en `.gitignore`) y no hay permisos POSIX (`core.filemode=false`).
`.gitattributes` fija `* text=auto` para neutralizar el ruido CRLF/LF — no
tocar esa configuración.
