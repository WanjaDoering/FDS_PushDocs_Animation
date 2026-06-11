# pushdocs — Animated Header Graphic (Dev Note)

**For:** FDS Dev Team
**Page:** Portfolio › pushdocs
**Placement:** directly **above** the section heading **"So funktioniert es"**

**Live preview:** https://wanjadoering.github.io/FDS_PushDocs_Animation/

---

## What this is

A self-contained, dependency-free animated header graphic (pure HTML + CSS + inline
SVG, no JS, no external libraries, no web fonts). It visualises the pushdocs flow:

```
Your Application  →  pushdocs  →  Target systems (4 categories)
```

- Left: a browser/app window mockup ("Ihre Anwendung") listing documents + metadata.
- Center: the pushdocs hub (brand SVG logo, soft pulsing ring).
- Right: 4 fixed category rows — **Buchhaltung, Cloud Storage, API & Webhook, Transfer** —
  each with 4 target-system logos (unified black silhouette).
- Animated dashed connector lines with blue dots flowing along them
  (1 inflow, 4 outflow S-curves; all outflows end on the same vertical line).

Responsive: below 760px viewport width the three columns stack vertically and the
connector SVG is hidden (it only makes sense in the horizontal layout).

---

## How to integrate

`index.html` contains **only the animation** plus a thin `.preview-shell` wrapper
(max-width + padding) for standalone preview.

1. Copy the `<style>` block into the page stylesheet. All selectors are namespaced
   under `.pushdocs-anim`, `.source-window`, `.target-row`, `.hub`, etc. —
   collisions are unlikely. The `.preview-shell` rule is optional (drop it if the
   page already provides a content container).
2. Copy the `<div class="pushdocs-anim">…</div>` element into the page, right
   above the "So funktioniert es" heading.
3. Copy the `assets/` folder (`assets/pushdocs.svg`, `assets/logos/*.svg`).
   Adjust the `src="assets/..."` references if assets live under a different path.

No build step, no JS.

---

## Notes / gotchas

- **Connector lines** are an absolutely-positioned `<svg class="lines" viewBox="0 0 1200 500">`
  with `preserveAspectRatio="none"`. All four outflow paths end at the same x (786), aligned
  in a vertical column just before the widest category label. Because label widths depend on
  the rendered font, if you swap the font you may need to nudge that end-x value (4 paths +
  4 `label-dot` cx values + the matching `offset-path` strings). They are commented in the SVG.
- **Flowing dots** use the CSS `offset-path: path(...)` property (Motion Path API). Supported
  in all current evergreen browsers. In older browsers the dots simply won't move — the
  lines still render.
- **Logo color**: target logos are forced to a single black silhouette via
  `filter: brightness(0)`. DATEV / Lexware / sevdesk are the official brand marks supplied
  by marketing; the rest are generic monochrome protocol icons (cloud, key, webhook, …).
- **Hub logo** `assets/pushdocs.svg` is the pushdocs brand mark in brand blue.

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
