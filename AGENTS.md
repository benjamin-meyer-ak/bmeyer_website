# bmeyer_website — Project Memory

## Overview

Hugo-based academic/research personal website for Benjamin Meyer (salmon habitat research, thermal imagery, Kenai River watershed focus). Uses the **Wowchemy v5** theme, deployed to **Netlify**, and live at https://www.benjamin-meyer.net/. Repository is on the `master` branch.

## Local Development Stack

- **Hugo:** 0.107.0, installed at `C:\Users\Benjamin\AppData\Roaming\Hugo\0.107.0\`
- **Netlify build:** Hugo 0.108.0, pinned in `netlify.toml` — do not change
- **R packages required:** `blogdown`, `rmarkdown`, `knitr`
- **`.Rprofile` settings:** `blogdown.knit.on_save = TRUE`, `blogdown.method = 'html'`
- **Go:** installed at `C:\Program Files\Go\bin\go.exe` — required by Hugo modules

## Editing Workflow

1. Use **Addins → New Post** (or `blogdown::new_post()`) to create content as `.Rmd` files.
2. blogdown auto-renders `.Rmd` → `.html` on save (knit-on-save is enabled).
3. **Commit both the `.Rmd` and the rendered `.html`** and push to `master`.
4. Netlify builds and deploys automatically.

> **Important:** Hugo is configured (in `config.yaml`) to ignore `.Rmd` files during the static build. Netlify only uses the rendered `.html`. This is the correct, intentional workflow — not a workaround.

## Starting a Session

```r
options(blogdown.server.timeout = 120)  # required — Hugo module verification takes >30s
library(blogdown)
blogdown::serve_site()  # local preview, port varies
```

> **Known issue:** `blogdown::serve_site()` times out with the default 30-second timeout because Hugo needs time to verify Go modules on startup. Setting `blogdown.server.timeout = 120` resolves this. Consider adding this option to `.Rprofile` to make it permanent.

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

- `config.yaml` — Main Hugo config; `baseurl` is `https://benjamin-meyer.net/` (no `www.`); `copyright` field currently reads `© 2025 Benjamin Meyer` (update to 2026 when convenient)
- `config/_default/menus.yaml` — Navigation menus; Resumé dropdown uses `identifier: tags` (placeholder — works fine as-is); dropdown includes Resumé (`uploads/resume.pdf`), CV (`uploads/CV_Meyer_October_2025.pdf`), and PhD Interest Letter (`uploads/letter_of_interest_meyer.pdf`)
- `config/_default/params.yaml` — Theme parameters
- `netlify.toml` — Netlify build config; Hugo version pinned to 0.108.0 here
- `go.mod` — Hugo module dependencies; Wowchemy v5 pinned to a specific commit hash

## Platform Status and Long-Term Plan

### Current platform: Wowchemy v5 (Hugo)
- The Wowchemy v5 theme is **deprecated** — superseded by Hugo Blox (same team, rebranded)
- The site is stable due to pinned Hugo (0.108.0) and pinned `go.mod` commit hash
- The Hugo module verification step is already causing local development friction (slow startup)
- **Do not aggressively update the theme** — Hugo Blox has migrated from Bootstrap to Tailwind; migration is a "start fresh" rebuild, not an upgrade
- **Do not invest in heavy Wowchemy v5 customization** — Bootstrap 4 code won't transfer to Quarto

### Future platform: Quarto website (planned migration)
- All other work (reports, books) is already in Quarto; a Quarto website is the natural long-term destination
- **Migration priority has been upgraded:** the deprecated theme is already causing local development friction, making an earlier transition more justified
- Migration is a real rebuild (not an upgrade) — plan as a dedicated future task, not an incremental one
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

### Planned content (from To Do list)

**Research/professional posts:**
- Comment on Lamborn 2025 Chinook manuscript
- Comment on global hatcheries assessment
- Value of TIR-based conservation: engage with JM argument (minimal value because conserving cool inputs does not change overall warming trend; RAD approach preferred) vs. recent literature on measurable value of cool stepping stones

**Project updates (priority: current work first; cross-post to KWF blog):**
- Thermal imagery
- AWC expansion (1st priority; new tool from ADFG and collaborators)
- BOR project
- Vogel Lake
- CAP

**Personal posts:**
- Bus project
- Dog pics
- Research fraud talk (slides: docs.google.com/presentation/d/1Z76-IkH0aYePEnL7rS-gunI3yMhAR8DKuQ8okWjyCq0)

## Key Notes

- Hugo versions between local (0.107.0) and Netlify (0.108.0) are close enough to not cause issues
- The site has been in regular use through at least September 2026
- Do not invest heavily in customizing the current Wowchemy v5 theme
