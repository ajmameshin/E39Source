# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Layout

This repo is a **Hugo theme** (`E39Source`, forked from the Universal theme) with a bundled example site:

```
/                        ← theme root (layouts, static, i18n, archetypes)
└── exampleSite/         ← the live E39Source.com site content and config
    ├── config.toml      ← site configuration (menus, params, integrations)
    ├── content/         ← markdown pages + blog posts
    ├── data/            ← YAML data for carousel, clients, testimonials, products
    ├── static/          ← site-specific static assets (img/, css/, wp-content/)
    └── public/          ← Hugo build output (do not edit)
```

The theme is referenced via `themesDir = "../.."` and `theme = "E39Source"`, so the theme root directory **is** the theme. Changes to `layouts/` or `static/` at the repo root affect the live site.

## Common Commands

All Hugo commands must be run from `exampleSite/`:

```bash
cd exampleSite

# Local dev server (live reload)
hugo server

# Production build
hugo

# Build with drafts
hugo server -D
```

Linting (run from repo root):

```bash
npm run stylelint   # CSS linting
npm run eslint      # JS linting
npm run prettier    # Format check
```

## Architecture

### Theme templates (`layouts/`)

- `index.html` — homepage; assembles landing page sections by calling partials in sequence
- `_default/single.html` — blog post template
- `page/single.html` — static page template (used by contact, about, service, etc.)
- `partials/` — reusable components; landing page sections (carousel, features, testimonials, clients, recent_posts) are each a separate partial driven by data files in `exampleSite/data/`
- `shortcodes/` — custom shortcodes: `shopping`, `stripe_checkout`, `bootstrap-table`, `bootstrap-table-right`, `product-table`

### Content types

Pages use `type = "page"` in front matter to route to `layouts/page/single.html`. Blog posts live in `content/blog/` and use the default single template. The contact page renders a Formspree form; the products page renders Stripe-integrated shopping via the `shopping` shortcode.

### Data-driven sections

Landing page sections are controlled by YAML files under `exampleSite/data/`:

| Section | Data path |
|---|---|
| Carousel | `data/carousel/*.yaml` |
| Clients | `data/clients/*.yaml` |
| Testimonials | `data/testimonials/*.yaml` |
| Products | `data/products.yaml` (prod: `products-prod.yaml`) |

### Integrations

- **Formspree** — contact form (`formspreeAction` in config.toml)
- **Stripe** — product purchases and donations via `stripe_checkout` shortcode
- **Google Maps** — footer map embed (currently disabled via `enableGoogleMaps = false`)
- **i18n** — 23 language files in `/i18n/`; default is `en-us`

### Hugo version compatibility

The site targets Hugo extended (Goldmark renderer, `unsafe = true` for raw HTML in markdown). The theme.toml states minimum v0.15, but current content relies on modern Hugo syntax — use a recent Hugo version (v0.100+).
