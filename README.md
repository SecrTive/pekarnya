# Pekarnya (Магазин выпечки)

Static landing page for a bakery located in Nefteyugansk, Russia. The page
is written in plain HTML and CSS with no build tooling, frameworks, or
JavaScript.

## Quick Start

No dependencies or build step required. Open the page directly in a browser:

```bash
# Option 1: open the file directly
open index.html.html        # macOS
xdg-open index.html.html     # Linux

# Option 2: serve with any static file server
python3 -m http.server 8000
# then visit http://localhost:8000/index.html.html
```

> **Note:** The main file is named `index.html.html` (double extension).
> Most static servers and web hosts expect `index.html`. See
> [Known Issues](#known-issues) below.

## Project Structure

```
.
├── index.html.html      # Main HTML page (see Known Issues re: filename)
└── css/
    └── style.css         # All site styles
```

### Page Sections

The HTML is organised into three content sections plus header and footer:

| Section     | Class prefix  | Content                                        |
|-------------|---------------|-------------------------------------------------|
| Header      | `<header>`    | Logo image (`img/01-Logo.svg`)                  |
| Navigation  | `<nav>`       | Links: О НАС, НОВОСТИ, МЕНЮ, КОНТАКТЫ (all `#`) |
| Section 1   | `.section-1`  | Hero banner with "Специальный заказ" heading    |
| Section 2   | `.section-2`  | Three icons + "Выпечка - это вкусно" heading   |
| Section 3   | `.section-3`  | "ЗАКАЗАТЬ" heading + Google Maps embed          |
| Footer      | `<footer>`    | Separator line + repeated navigation links      |

### CSS Architecture

All styles live in `css/style.css` (134 lines). Naming follows a
section-based convention reminiscent of BEM:

- `.section-N` — top-level section container
- `.section-N__element` — child element within a section
- `.section-N__element--modifier` — (no modifiers currently used)

Fonts are loaded via `@font-face` declarations referencing local font
files in a `fonts/` directory (see [Missing Assets](#missing-assets)).

#### Color Palette

| Colour      | Hex / value                              | Used for                         |
|-------------|------------------------------------------|----------------------------------|
| Warm cream  | `#ffe2b0`                                | Body background                  |
| Coral       | `#f15b40`                                | `.section-2__span` accent        |
| Gray        | `#999999`                                | Footer separator line            |
| Gradient    | `rgba(244,213,178,0.8)` → `rgba(234,156,65,0.8)` | `.section-1__block` overlay |
| Black       | `black`                                  | Text, borders                    |
| Red         | `red`                                    | Link hover state                 |

#### Font Usage

| Selector              | Font         | Notes                                  |
|-----------------------|--------------|----------------------------------------|
| `html` (default)      | Roboto       | Applied to all text unless overridden  |
| `.section-1__title`   | Philosopher  | Hero heading ("Специальный заказ")     |
| `.section-2__title`   | Philosopher  | "Выпечка - это вкусно"                 |
| `.section-3__title`   | Philosopher  | "ЗАКАЗАТЬ"                             |

## Local Development

1. Clone the repository.
2. Open `index.html.html` in a browser or serve with a static server.
3. Edit `index.html.html` for markup changes or `css/style.css` for
   styling changes.
4. Refresh the browser to see changes (no hot reload).

No `package.json`, no linters, and no test suite exist in this project.

## Deployment

The site is fully static — no build step — so it can be hosted on any
static-file provider. Before publishing, resolve the
[double-extension filename](#1-double-file-extension-on-main-page), since
most hosts serve `index.html` by default:

```bash
git mv index.html.html index.html
```

> The assets referenced from `img/`, `icons/`, and `fonts/` are not in the
> repository (see [Missing Assets](#missing-assets)). Add them before
> publishing, or images and fonts will be broken on the live site.

### GitHub Pages

1. Push `main` to GitHub.
2. Open **Settings → Pages** for the repository.
3. Set **Source** to **Deploy from a branch**, pick `main` and the
   `/root` directory, then save.
4. The site publishes to `https://<owner>.github.io/pekarnya/`.

### Other static hosts

Upload the repository contents (after the rename above) to the web root
of any static host — Netlify, Vercel, Cloudflare Pages, S3 + CloudFront,
etc. No build command and no output directory are needed.

## Missing Assets

The HTML and CSS reference several files that are **not included** in the
repository. These missing assets cause broken images, unstyled fonts, and
missing CSS resets:

| Referenced file               | Referenced in    | Type     |
|-------------------------------|------------------|----------|
| `css/reset.css`               | `index.html.html`| CSS      |
| `css/normalize.css`           | `index.html.html`| CSS      |
| `img/01-Logo.svg`             | `index.html.html`| Image    |
| `icons/01-Иконка.svg`         | `index.html.html`| Icon     |
| `icons/02-Иконка.svg`         | `index.html.html`| Icon     |
| `icons/03-Иконка.svg`         | `index.html.html`| Icon     |
| `img/01-picture fone.jpg`     | `css/style.css`  | Image    |
| `fonts/Philosopher-Regular.ttf`| `css/style.css` | Font     |
| `fonts/Roboto-Regular.eot`    | `css/style.css`  | Font     |
| `fonts/Roboto-Regular.woff`   | `css/style.css`  | Font     |
| `fonts/Roboto-Regular.ttf`    | `css/style.css`  | Font     |

**Impact:**
- The hero section (`.section-1`) will have no background image.
- The logo and section-2 icons will show broken image placeholders.
- The `Philosopher` and `Roboto` font families fall back to system defaults.
- Without `reset.css` / `normalize.css`, browser default styles may cause
  minor layout inconsistencies.

**To fix:** Add the missing files to their referenced paths, or replace
local font references with a CDN (e.g., Google Fonts for Roboto and
Philosopher).

## Known Issues

### 1. Double file extension on main page

The main HTML file is named `index.html.html`. Most web servers and hosting
platforms serve `index.html` by default. Rename the file to `index.html`
for standard deployment:

```bash
git mv index.html.html index.html
```

### 2. Navigation links are placeholders

All `<nav>` links point to `#` (no actual targets). The footer navigation
duplicates the same placeholder links.

### 3. Hardcoded Google Maps embed

Section 3 contains a Google Maps `<iframe>` with an embed URL hardcoded to
a specific location. If the bakery moves, the embed URL must be updated
manually in `index.html.html` (line 53).

### 4. Fixed-width layout

Several elements use fixed pixel widths (e.g., `.section-2__text2` at
`width: 1000px`, `.line` at `width: 1024px`). The layout is not responsive
and will overflow on viewports narrower than ~1024px.

The `<meta name="viewport" content="width=device-width, initial-scale=1.0">`
tag (`index.html.html`, line 6) declares mobile-friendly intent, but the
fixed-width CSS contradicts it: `width=device-width` sets the layout
viewport to the device width (e.g., ~390px on a phone), so the 1000–1024px
blocks simply **overflow** it. The page requires horizontal panning — the
browser does *not* scale the layout down to fit, because shrink-to-fit
only happens when a viewport meta tag is missing.

### 5. Incorrect `lang` attribute

The `<html>` tag declares `lang="en"` (`index.html.html`, line 2), but every
piece of visible content — the title, navigation, headings, and body copy —
is in Russian. An incorrect `lang` value has two effects:

- **Accessibility:** screen readers use `lang` to pick pronunciation rules,
  so Russian text is read with English phonetics and becomes garbled.
- **SEO:** search engines rely on `lang` for language detection and
  localized ranking, so the page may be misclassified.

Fix it by setting the attribute to Russian:

```html
<html lang="ru">
```

### 6. Misspelled `alt` text on logo image

The logo `<img>` declares `alt="Логотоип"` (`index.html.html`, line 16).
The correct Russian word for "logo" is **Логотип** — the sixth character
should be `и`, not `о`. This is a visible typo in accessibility tools and
screen-reader output.

Fix:

```html
<img src="img/01-Logo.svg" alt="Логотип">
```

### 7. Placeholder filler text in content sections

Both Section 1 (`.section-1__text`, line 31) and Section 2
(`.section-2__text2`, line 45) contain generic bureaucratic Russian
boilerplate ("Товарищи! рамки и место обучения кадров…") that has nothing
to do with a bakery. This is template filler that was never replaced with
real content. Replace both paragraphs with actual bakery copy before
publishing.

### 8. Redundant `h1` CSS rule

`css/style.css` lines 132–134 set `h1 { font-family: "Roboto"; }`, but
this rule is dead code:

- `html` already sets `font-family: 'Roboto'` as the default (line 5).
- The only `<h1>` on the page has class `.section-1__title`, which sets
  `font-family: 'Philosopher'` (line 81) — a more specific selector that
  wins.

The `h1` block can be removed without any visual change.

### 9. Hero overlay block is not horizontally centered

`.section-1__block` (the semi-transparent gradient overlay containing the
"Специальный заказ" heading) is positioned with `left: 50%` but lacks
`transform: translateX(-50%)` (`css/style.css`, line 63). This places the
**left edge** of the 420px-wide block at the 50% mark, shifting the entire
overlay ~210px to the right of true center.

Fix:

```css
.section-1__block {
    /* existing properties… */
    transform: translate(-50%, -50%);
}
```

Adding `translateX(-50%)` (or `-50%, -50%` if vertical centering is also
desired) shifts the block left by half its own width, centering it
properly within the hero section.

### 10. Broken `local()` fallback in Philosopher `@font-face`

The `@font-face` declaration for Philosopher (`css/style.css`, lines 8–12)
uses two separate `src` properties:

```css
src: local("Philosopher");
src: url(../fonts/Philosopher-Regular.ttf);
```

In modern browsers the second `src` declaration **overwrites** the first,
so `local("Philosopher")` is never evaluated. Even if the user has
Philosopher installed system-wide, the browser skips the local copy and
attempts to download the `.ttf` file (which is missing from the repo — see
[Missing Assets](#missing-assets)). The Philosopher `font-family` lists on
the headings have no fallback families, so the result is a silent fallback
to the browser's default font for all heading text.

Fix by combining both sources in a single `src` property:

```css
@font-face {
    font-family: "Philosopher";
    src: local("Philosopher"),
         url(../fonts/Philosopher-Regular.ttf);
}
```

With a comma-separated list the browser tries `local()` first and falls
back to the URL only if the local font is unavailable.

### 11. Overly broad `:last-child` selector in section-2 icon row

The CSS rule that removes the right margin from the last icon
(`css/style.css`, line 94) uses a **descendant combinator** — the space
between `.section-2__pic` and `:last-child`:

```css
.section-2__pic  :last-child{
    margin-right: 0px;
}
```

This matches *any* element that is a `:last-child` anywhere inside
`.section-2__pic`, not just the last `<img>`. It works today because the
only children are three `<img>` elements, but it is fragile: if a
non-image element (e.g., a `<span>` caption) is added after the icons,
the selector would match that element instead, and the last icon would
keep its `76px` right margin.

Fix by scoping the selector to the last image:

```css
.section-2__pic img:last-child {
    margin-right: 0px;
}
```

Or use the direct-child combinator to limit depth:

```css
.section-2__pic > :last-child {
    margin-right: 0px;
}
```
