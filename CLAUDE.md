# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Proyecto

Portafolio personal construido sobre la plantilla HTML "Genz" de AliThemes: sitio estático con Pug + SCSS, compilado con Gulp 4. No hay framework, backend, tests ni linter.

El texto de la plantilla sigue en inglés y con datos de ejemplo. La rama `plantilla-limpia` conserva la plantilla base; el portafolio se trabaja en `version1`.

## Comandos

```bash
npm run build   # gulp: borra dist/ y regenera HTML, CSS y assets
npm run dev     # gulp serve: Browsersync sobre dist/ + watchers
```

- `npm run dev` no compila antes de servir: si `dist/` no existe o está desactualizado, ejecuta primero `npm run build`.
- Node está en `C:\Program Files\nodejs`, pero puede faltar en el PATH de la sesión. En PowerShell: `$env:Path = "C:\Program Files\nodejs;$env:Path"`.

## Arquitectura

**Compilación.** Cada `src/views/pages/*.pug` genera un `dist/<nombre>.html`, sin subcarpetas; por eso todos los enlaces son relativos y planos (`href='page-about.html'`). `dist/` está en `.gitignore`: los cambios se hacen siempre en `src/`.

**Vistas.** Todas las páginas extienden `layouts/master.pug`, que incluye los mixins, el preloader, el menú móvil, el footer y los scripts. Cada página rellena los bloques `title`, `header` y `content`, y compone el contenido con includes de:
- `sections/`: bloques de contenido agrupados por página.
- `components/`: piezas globales (header, menús, footer, sidebars).
- `elements/`: piezas pequeñas (breadcrumbs, cabeceras, paginación).
- `mixins/mixins.pug`: tarjetas de blog y otros mixins reutilizables, con valores de ejemplo por defecto.

**Home.** `index.pug` usa las secciones de `sections/homepage2/`, y el enlace Home del menú va directo a `index.html`, sin submenú. `index-2.pug` genera la misma página.

**Secciones compartidas.** Las carpetas `sections/homepageN/` no son exclusivas de su home. Antes de borrar o editar una sección, busca sus includes:
- `homepage1/banner.pug`: `page-about`
- `homepage1/popular_tags.pug`: `blog-archive`, `-2` y `-5`
- `homepage3/partners.pug`: `page-about`, `page-portfolio`, `page-portfolio-2`
- `homepage3/sidebar_bottom.pug`: `blog-archive-4`
- `homepage4/banner.pug`: `page-author`

Lo mismo pasa con las imágenes de `assets/imgs/page/homepage*`, que se usan en muchas vistas.

**Menús duplicados.** La navegación está en dos archivos independientes, `components/main-menu.pug` (escritorio) y `components/mobile-menu.pug` (móvil). Cualquier cambio de navegación se aplica en ambos.

**Estilos.** `src/assets/scss/style.scss` es el único punto de entrada e importa los parciales. Compila a `src/assets/css/style.css`, que está versionado (junto con su `.map`) y se copia a `dist/`. Nunca edites `style.css` a mano.

**Tema día/noche.** El tema por defecto es oscuro. `layout/_dark.scss` define las variantes bajo `.theme-day`, y `assets/js/main.js` alterna el tema y lo guarda en `localStorage`. Un color nuevo necesita su variante para ambos temas. El logo tiene dos imágenes, `.logo-night` y `.logo-day`.

**JavaScript.** Todo el código propio está en `assets/js/main.js`; las librerías (Bootstrap, jQuery, Swiper, etc.) están en `assets/js/vendors/` y se cargan desde `components/script-vendors.pug`. Gulp no empaqueta JS: lo copia tal cual.

## Convenciones

- Los `.pug` mezclan indentación con tabs y con espacios según el archivo. Respeta la del archivo que edites, porque Pug falla si se mezclan.
