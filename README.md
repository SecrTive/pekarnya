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

## Local Development

1. Clone the repository.
2. Open `index.html.html` in a browser or serve with a static server.
3. Edit `index.html.html` for markup changes or `css/style.css` for
   styling changes.
4. Refresh the browser to see changes (no hot reload).

No `package.json`, no linters, and no test suite exist in this project.

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
