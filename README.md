# Pekarnya — bakery landing page

Static, dependency-free landing page for a bakery in Nefteyugansk ("Магазин выпечки"). Plain HTML + CSS: no JavaScript, no build step, no package manager.

## Repository contents

| Path | Purpose |
|------|---------|
| `index.html.html` | The entire page (single-file landing) |
| `css/style.css` | All styling (fonts, layout, colors) |

## Page structure

The page is one `<main>` with three sections, plus header/footer navigation:

1. **Header** — logo image, centered.
2. **Nav** (header and footer) — four placeholder links (`О НАС`, `НОВОСТИ`, `МЕНЮ`, `КОНТАКТЫ`), all pointing to `#`. No target pages/anchors exist yet.
3. **`section-1`** — hero. Background photo with an absolutely positioned gradient card ("Специальный заказ").
4. **`section-2`** — three icon images and the "Выпечка — это вкусно" heading with an accent-colored span (`#f15b40`).
5. **`section-3`** — "ЗАКАЗАТЬ" heading with a 900×400 Google Maps `<iframe>` embed (bakery location).
6. **Footer** — 1px divider line (`.line`, 1024px wide) and a repeat of the nav.

## Running locally

Any static file server works. From the repo root:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Or use the VS Code "Live Server" extension. There is nothing to build or install.

## Deployment notes

- **Filename pitfall:** the page is named `index.html.html` (double extension). Static hosts (GitHub Pages, Netlify, nginx `index` directive) resolve the default document to `index.html`, so `/` returns 404 until the file is renamed to `index.html`.
- **Asset filenames use Cyrillic** (`icons/01-Иконка.svg`). Some servers, CI pipelines, and zip tools mishandle non-ASCII paths; ASCII filenames are safer.
- **Fixed widths:** the map iframe (900px), the section-2 paragraph (1000px), and the footer line (1024px) are hard-coded in px. The viewport meta tag is present, but nothing reflows below ~1024px — treat this as a desktop-only layout.

## Known gaps (verified against the repo)

The repository contains only `index.html.html` and `css/style.css`. The following are referenced by the markup/CSS but **missing from the repo**, so they 404 at runtime:

| Missing | Referenced from |
|---------|-----------------|
| `css/reset.css`, `css/normalize.css` | `index.html.html` `<head>` |
| `img/01-Logo.svg` | header `<img>` |
| `img/01-picture fone.jpg` | `.section-1` background in `style.css` |
| `icons/01-Иконка.svg`, `icons/02-Иконка.svg`, `icons/03-Иконка.svg` | `section-2` `<img>` tags |
| `fonts/Philosopher-Regular.ttf`, `fonts/Roboto-Regular.{eot,woff,ttf}` | `@font-face` rules in `style.css` |

Consequences:

- Missing icon/logo/photo files render as broken images (or no hero background). The hero gradient card still displays because it is CSS-only.
- Fonts fall back to the browser default; the `font-family: 'Roboto'` / `'Philosopher'` declarations are inert until the `fonts/` files are added.
- To make the site fully render, add an `img/`, `icons/`, and `fonts/` directory with the files above (keeping the exact paths), or update the references.

## CSS conventions

- BEM-style class names: `section-1__title`, `section-2__pic` — block (`section-N`) + element (`__name`). There are no modifiers or `--` variants yet.
- Typography: Philosopher for headings, Roboto for body text, both via `@font-face`.
- Palette: page background `#ffe2b0` (cream), accent `#f15b40`, hero gradient `rgba(244,213,178,.8) → rgba(234,156,65,.8)`.
- Global `box-sizing: border-box` is set via `*`.
