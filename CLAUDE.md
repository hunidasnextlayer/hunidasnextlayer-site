# CLAUDE.md — Hunidas NextLayer IT Website

## Project Overview

This is the official website for **Hunidas NextLayer IT**, a Portuguese IT services and consulting company. It is a **static single-page website** with no build process, framework, or backend. All content is in Portuguese (pt-PT).

- **Live URL:** https://hunidasnextlayer.pt
- **Hosting:** GitHub Pages (configured via `CNAME` file)
- **Language:** Portuguese (pt-PT)

---

## Repository Structure

```
hunidasnextlayer-site/
├── index.html          # Entire website — single HTML file
├── index.html_v1       # Previous version backup (kept for reference)
├── sitemap.xml         # XML sitemap for SEO
├── robots.txt          # Search engine crawl rules
├── CNAME               # GitHub Pages custom domain (hunidasnextlayer.pt)
├── favicon.ico         # Browser tab icon
├── fundo_bk1.jpg       # Hero section background image
├── hunidas.png         # Logo/brand image
├── icone_fundo.png     # Icon variant
├── icone_fundo_1.png   # Primary spinning icon used in header and footer
├── icone_fundo_1_old.png  # Old icon version
├── preview1.png        # Social preview image (root)
└── img/
    ├── preview.png     # OG/social preview image (referenced in <meta og:image>)
    └── preview1.png    # Additional preview
```

---

## Technology Stack

All dependencies are loaded from CDNs — there is no `package.json`, no build step, and no local `node_modules`.

| Library | Version | Purpose |
|---|---|---|
| [Tailwind CSS](https://tailwindcss.com) | CDN (latest) | Utility-first CSS framework |
| [Phosphor Icons](https://phosphoricons.com) | `@phosphor-icons/web` via unpkg | Icon set (`ph ph-*` classes) |
| [Swiper.js](https://swiperjs.com) | v11 via jsDelivr | Blog article carousel |
| [Calendly Widget](https://calendly.com) | External script | Meeting booking integration |

---

## Page Sections (Anchor IDs)

The site is a single-page layout with smooth-scroll navigation. Sections in order:

| ID | Nav Label | Description |
|---|---|---|
| `#hero` | — | Full-screen hero with background image and CTA buttons |
| `#sobre` | Sobre | About the company |
| `#servicos` | Serviços | 5 service cards (grid layout) |
| `#competencias` | Competências | Accordion-based technical skills list |
| `#blog` | Blog | Swiper.js carousel linking to LinkedIn posts |
| `#impacto` | Impacto | Accordion describing client impact |
| `#valores` | Valores | Accordion describing company values |
| `#contacto` | Contacto | Email link + inline Calendly booking widget |

---

## Key Conventions

### HTML Structure
- Everything lives in `index.html` — a single self-contained file.
- Styles are split between two `<style>` blocks in `<head>` (animations, Calendly sizing) and inline Tailwind utility classes.
- JavaScript is written inline in `<script>` tags, placed near the bottom of `<body>` or adjacent to the feature they control.
- **There is no module system.** All JS is plain vanilla, in the global scope.

### CSS / Tailwind
- Uses the Tailwind CDN build (not a compiled config). This means no `tailwind.config.js` — all classes are available.
- Custom animations are defined with `@keyframes` in `<style>` blocks:
  - `.animate-spin-slow` — 20s slow rotation (used on the header icon)
  - `.animate-glow` — pulsing drop-shadow glow effect
- Color theme: **dark background** (`bg-gray-950`), **cyan accent** (`text-cyan-400`, `bg-cyan-500`).

### JavaScript Patterns
- **Mobile menu toggle:** `#menu-toggle` button toggles `hidden` class on `#nav-menu`. Clicking a nav link or outside the menu also closes it.
- **Accordion toggle:** `toggleAccordion(id)` function toggles `hidden` class and switches icon between `ph-plus-circle` / `ph-minus-circle`. **Note:** `toggleAccordion` is defined twice in the file — this is a known bug (the second definition overrides the first).
- **"Ver Todas" button:** Expands/collapses all competências accordions simultaneously.
- **Calendly inline widget:** `toggleCalendly()` shows/hides `#calendly-widget` with a CSS opacity fade.
- **Back-to-top button:** `#backToTop` appears after scrolling 300px and scrolls to top on click.
- **Swiper blog carousel:** Initialised on `DOMContentLoaded`, auto-plays every 4s, pauses on hover. Responsive breakpoints: 1 slide (<768px), 2 slides (768px+), 3 slides (1024px+).
- **Calendly badge widget (desktop only):** Initialised on `window.load` only if `window.innerWidth >= 640`.
- **Calendly popup button (mobile only):** A fixed button visible only on mobile (`block sm:hidden`).

### SEO & Meta
- `<meta name="google-site-verification">` is present — do not remove.
- `<link rel="canonical">` points to `https://hunidasnextlayer.pt/`.
- Open Graph tags use `img/preview.png` for social sharing.
- `sitemap.xml` lists all anchor sections with priorities and change frequencies.

---

## Development Workflow

### Editing the Site
1. Edit `index.html` directly — there is no compilation step.
2. Open the file in a browser to preview changes locally.
3. Commit and push to `master` (or `main`) — GitHub Pages will deploy automatically.

### No Build Process
- Do **not** introduce `npm`, build tools, or framework dependencies unless explicitly requested.
- Do **not** split `index.html` into components or templates unless migrating the entire site.

### Version Backup
- `index.html_v1` is the previous major version. Do not delete it unless asked.
- Before making large structural changes, consider copying `index.html` to a new versioned backup.

### Branch Strategy
- Production is served from the default branch (`master` / `main`).
- Feature work and AI-assisted changes use `claude/`-prefixed branches (e.g. `claude/add-claude-documentation-gcfBC`).

---

## Contact & Integrations

- **Email:** corporate@hunidas.pt
- **Calendly:** https://calendly.com/hunidasnextlayer
- **LinkedIn:** Referenced in blog posts (author: davidjesus)
- **Google Search Console:** Verified via meta tag

---

## Known Issues / Technical Debt

- `toggleAccordion()` is defined **twice** (lines ~759–765 and ~770–776). The second definition shadows the first. Safe to remove the first duplicate.
- Calendly CSS and JS are loaded **twice** (once near `#contacto`, once near the end of `</body>`). The duplicate `<link>` and `<script>` tags should be deduplicated.
- The `<style>` block for `.calendly-badge-widget` also appears twice with slightly different `transform: scale()` values (`0.85` in `<head>`, `0.9` near end of body).
- All `<script>` tags are render-blocking or unoptimized — consider adding `defer` or `async` where applicable.
