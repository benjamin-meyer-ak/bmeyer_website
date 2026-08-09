# bmeyer_website — Project Memory

## Overview

Hugo-based academic/research personal website for Benjamin Meyer (salmon habitat research, thermal imagery, Kenai River watershed focus). Uses the **Wowchemy v5** theme, deployed to **Netlify**, and live at https://www.benjamin-meyer.net/. Repository is on the `master` branch.

## Local Development Stack

- **Hugo:** 0.107.0, installed at `C:\Users\Benjamin\AppData\Roaming\Hugo\0.107.0\`
- **Netlify build:** Hugo 0.108.0, pinned in `netlify.toml` — do not change
- **R packages required:** `blogdown`, `rmarkdown`, `knitr`
- **`.Rprofile` settings:** `blogdown.knit.on_save = TRUE`, `blogdown.method = 'html'`

## Editing Workflow

1. Use **Addins → New Post** (or `blogdown::new_post()`) to create content as `.Rmd` files.
2. blogdown auto-renders `.Rmd` → `.html` on save (knit-on-save is enabled).
3. **Commit both the `.Rmd` and the rendered `.html`** and push to `master`.
4. Netlify builds and deploys automatically.

> **Important:** Hugo is configured (in `config.yaml`) to ignore `.Rmd` files during the static build. Netlify only uses the rendered `.html`. This is the correct, intentional workflow — not a workaround.

## Starting a Session

```r
library(blogdown)
blogdown::serve_site()  # local preview at localhost:4321
```

If `blogdown` is missing (can happen after R upgrades), reinstall with:

```r
install.packages("blogdown")
```

## Content Structure

- `content/project/` — Research project pages (`.Rmd` + rendered `.html`)
- `content/post/` — Blog posts
- `content/talk/` — Talks/presentations
- `content/publication/` — Publications
- `content/home/` — Homepage widget markdown files
- `static/` — Static assets (images, PDFs, etc.)

There are 12 `.Rmd` files in the repository, each with a corresponding `.html` sibling.

## Configuration Files

- `config.yaml` — Main Hugo config
- `config/_default/menus.yaml` — Navigation menus (note: Resumé dropdown `identifier` field has a placeholder comment; works fine as-is)
- `config/_default/params.yaml` — Theme parameters
- `netlify.toml` — Netlify build config; Hugo version pinned to 0.108.0 here
- `go.mod` — Hugo module dependencies; Wowchemy v5 pinned to a specific commit hash

## Platform Status and Long-Term Plan

### Current platform: Wowchemy v5 (Hugo)
- The Wowchemy v5 theme is **deprecated** — superseded by Hugo Blox (same team, rebranded)
- The site is stable due to pinned Hugo (0.108.0) and pinned `go.mod` commit hash
- Expected to remain functional for several more years without intervention
- **Do not aggressively update the theme** — Hugo Blox has migrated from Bootstrap to Tailwind; migration is a "start fresh" rebuild, not an upgrade

### Future platform: Quarto website
- All other work (reports, books) is already in Quarto; a Quarto website is the natural long-term destination
- Transition is planned for when necessary (redesign, theme breakage, etc.) — not urgent
- All content (Markdown files) is portable; the content is the durable asset

## Planned Additions (Next Priority Tasks)

### Personal blog section
- A writing-focused blog section (distinct from research `content/post/` or `content/project/`) 
- All posts free and public

### Newsletter: Brevo
- **Service:** Brevo (formerly Sendinblue) — free tier, unlimited contacts, 9,000 emails/month
- **Workflow:** Embed subscribe form on Hugo site; manually send emails from Brevo dashboard when desired (not auto-sent on every post — Benjamin chooses per post)
- **Integration:** Subscribe form is plain HTML embed; no Hugo config changes required

### Donation link: Ko-fi or Buy Me a Coffee
- Simple link or embeddable button widget
- Just HTML — can go anywhere on the site

## Key Notes

- Hugo versions between local (0.107.0) and Netlify (0.108.0) are close enough to not cause issues
- The site has been in regular use through at least May 2026
- Do not invest heavily in customizing the current Wowchemy v5 theme — Bootstrap 4 code won't transfer to Hugo Blox or Quarto
