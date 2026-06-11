# pushdocs — Animated Header Graphic (Dev Note)

**For:** FDS Dev Team
**Page:** Portfolio › pushdocs
**Placement:** directly **above** the section heading **"So funktioniert es"**

---

## What this is

A self-contained, dependency-free animated header graphic (pure HTML + CSS + inline
SVG, no JS, no external libraries). It visualises the pushdocs flow:

```
Your Application  →  pushdocs  →  Target systems (4 categories)
```

- Left: a browser/app window mockup ("Ihre Anwendung") emitting documents + metadata.
- Center: the pushdocs hub (brand SVG logo, soft pulsing ring).
- Right: 4 fixed category rows — **Buchhaltung, Cloud Storage, API & Webhook, Transfer** —
  each with 4 real target-system logos (unified black silhouette).
- Animated dashed connector lines with blue dots flowing along them
  (1 inflow path, 4 outflow S-curves).

Everything is responsive: on screens ≤ 760px the three columns stack vertically and the
connector SVG is hidden (it only makes sense in the horizontal layout).

---

## How to integrate

The animation lives between these two markers in `index.html`:

```html
<!-- ANIMATION BLOCK START — copy from here for embed -->
...
<!-- ANIMATION BLOCK END -->
```

Everything **above** the START marker (page header, breadcrumb, hero) is **mockup only** —
it exists so the graphic can be previewed in context. **Do not copy it.**

### Steps
1. Copy the `<style>` block from `<head>` into the page's stylesheet (or keep it inline /
   scoped — all selectors are namespaced under `.pushdocs-anim`, `.source-window`,
   `.target-row`, `.hub`, etc., so collisions are unlikely).
2. Copy the markup **between the ANIMATION BLOCK markers** into the page, right above the
   "So funktioniert es" heading. (The block already contains that heading + intro — keep or
   drop as you prefer; if the page already renders that heading, delete the duplicated
   `<h2>` / `<p>` inside the block.)
3. Copy the `assets/` folder (relative paths: `assets/pushdocs.svg`,
   `assets/logos/*.svg`). Adjust the `src="assets/..."` and `offset-path` references if you
   host assets under a different path.

No build step, no fonts to load (uses the system UI font stack), no JS.

---

## Notes / gotchas

- **Connector lines** are an absolutely-positioned `<svg class="lines" viewBox="0 0 1200 500">`
  with `preserveAspectRatio="none"`. All four outflow paths end at the same x (786), aligned
  in a vertical column just before the widest category label. Because label widths depend on
  the rendered font, if you swap the font you may need to nudge that end-x value (4 paths +
  4 `label-dot` cx values + the matching `offset-path` strings). They are commented in the SVG.
- **Flowing dots** use the CSS `offset-path: path(...)` property (Motion Path API). Supported
  in all current evergreen browsers (Chrome/Edge/Safari/Firefox). No fallback needed for
  modern targets; if an old browser is in scope the dots simply won't move (lines still show).
- **Logo color**: target logos are forced to a single black silhouette via
  `filter: brightness(0)` so mixed-source logos read consistently. The DATEV / Lexware /
  sevdesk SVGs are the official brand marks supplied by marketing; the rest are generic
  monochrome icons (cloud, key, webhook, server, …) representing protocols.
- **Hub logo** `assets/pushdocs.svg` is the brand mark in FDS blue (#2e9bcc as delivered).

---

## Brand tokens used

| Token            | Value     |
|------------------|-----------|
| Text             | `#060608` |
| Button blue      | `#003B86` |
| Grey (UI lines)  | `#DDE1EA` |
| Surface grey     | `#F7F7F7` |
| Surface white    | `#FFFFFF` |

Connector dots use the button blue (`#003B86`).
