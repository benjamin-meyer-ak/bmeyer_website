# bmeyer_website: Project Memory

## Overview

Personal academic/research website for Benjamin Meyer (salmon habitat research, thermal imagery, Kenai River watershed focus). Live at https://www.benjamin-meyer.net/, deployed on **Netlify**.

**Migration in progress (started Oct 22, 2026):** the live site is still Hugo + Wowchemy v5 on `master`. A Quarto rebuild lives on the branch `quarto-migration` (latest commit `50dd92a` as of Oct 31, includes the projects restructure and Hugo cleanup). The branch was pushed to `origin/quarto-migration` with upstream tracking on Oct 31 (the first push timed out in a background shell; run it from the RStudio Terminal so credential prompts can appear), so RStudio's Push/Pull buttons now work. `master` and the live site are untouched until cutover.

**Deployment safety:** production deploys from `master`, whose `netlify.toml` still builds Hugo. Never merge `quarto-migration` into `master` or make it Netlify's production branch until the Quarto site is ready for public viewing. If pushing the branch, enable Netlify branch deploys for `quarto-migration` only (not "All"); consider a `noindex` meta tag for the preview.

Style: avoid em dashes and AI writing tropes, use the base pipe `|>`, keep code comments minimal.

## Quarto Migration (branch `quarto-migration`)

Renders cleanly with `quarto render` (Quarto 1.6.40) from the project root; output goes to `_site`.

### Structure
- `_quarto.yml`: website type, cosmo theme, `site-url: https://www.benjamin-meyer.net`, `resources: uploads/**`. An explicit `render:` list (`index.qmd`, `publications.qmd`, `talks.qmd`, `blog/index.qmd`, and `blog/posts`, `projects`, `talks`, `publications` subfolders) so only listed paths render.
- Navbar: Home, Blog, Projects dropdown (Research, Other), Talks, Publications, Resume/CV dropdown (Resume, `CV_Meyer_October_2025.pdf`, PhD Interest Letter), GitHub and LinkedIn icons.
- Footer: "© 2026 Benjamin Meyer" plus a "Support on Ko-fi" link.
- `static/uploads/` was moved to `uploads/` (git mv) so URLs stay `/uploads/...`. Avatar is at `images/avatar.jpg`.
- `index.qmd`: jolla about-template (bio and social links only). Skills/experience were tried and removed (covered by the Resume/CV); the Education list was dropped too and survives in `_archive/hugo-content/authors/admin/_index.md`.
- `talks.qmd`, `publications.qmd`, `blog/index.qmd`, `projects/research/index.qmd`, `projects/other/index.qmd`: listing pages, sorted by date descending. Old `projects.qmd` was removed.
- `_includes/subscribe.html`: Brevo iframe embed, pulled into `blog/index.qmd` via an include inside a raw HTML block (the bare include gave an "unclosed div" warning). An inline script in `blog/index.qmd` moves `.subscribe-box` to the end of `#quarto-margin-sidebar` on load so it sits below the categories list (a `.column-margin` wrapper put it above the categories). `page-layout: full` was removed from the blog index because it removes the margin column.
- `_includes/kofi.html`: Ko-fi button, currently unused.
- `netlify.toml` (on this branch only): downloads Quarto 1.6.40 from GitHub in the build command, runs `quarto render`, publishes `_site`. No Hugo pins or cache plugin. Not yet tested on Netlify; if the build fails, check the `$HOME/quarto-1.6.40/bin/quarto` path. No qmd pages contain R/Python chunks, so R is not needed on Netlify.

### Where to edit (Quarto)
- Landing page: `index.qmd` (bio below the YAML header; social links in the header).
- Navbar, footer, theme: `_quarto.yml`.
- Posts, projects, talks, publications: `<section>/<name>/index.qmd`. Listing pages (`blog/index.qmd`, `talks.qmd`, `publications.qmd`) only set layout.
- Uploads (PDFs etc.): `uploads/`.
- Hugo/Wowchemy files were removed from this branch on Oct 31, 2026 (still on `master` and in git history). Unported material (homepage skills/experience widgets, drafts, AWC draft docs, draft post text files, brand logos) is in `_archive/`, which is not rendered.
- Preview: `quarto preview` in an RStudio terminal (ports used: 4848, 4666). The server does not notice folders moved with `git mv`; restart it after moving or renaming content, otherwise listings and `listings.json` stay stale.

### Content ported
- **Blog (`blog/posts/`):** only `a-visit-to-skilak-glacier` is a real post (uses `references.bib`, about 24 MB of photos). Four other former posts were converted to research projects (below). Each ported item has an `aliases:` entry for its old URL. Wowchemy example posts were skipped.
- **Placeholders to delete once real posts exist:** `welcome` (draft: true) and `example-post-one/two/three` (lorem ipsum, category `example`).
- **Projects:** `projects/research/` holds `chena-juv-salmon-wildfire` (alias guessed, verify), `kwf-news`, `climate-change-and-juvenile-salmon`, `as-local-streams-warm` (KDLL radio, a stub that needs finishing), `stream-by-stream-volunteers-map`, `scientific-collaboration`. Moving a folder one level deeper breaks relative links (fixed one to the thesis publication). `projects/other/` holds the ported Hugo `other` posts (balloon guy, wild salmon twitter bot, cabbage limerick contest, spruce tip). Aliases point to old URLs.
- **Talks:** `krga-salmon-lifecycle`, `hows-the-water`, `unmapped-salmon-habitat`.
- **Publications:** 10 pages generated by an R script from Hugo metadata, with local PDFs where available (about 46 MB). Eight old records had lorem ipsum abstracts, which were omitted; only the 2023 Kenai paper and the 2009 polar bear mercury paper have real abstracts.
  - Schoen 2017: DOI 10.1080/03632415.2017.1374251, Taylor & Francis link.
  - Gouin 2010: DOI 10.5094/APR.2010.002, ScienceDirect pii S1309104215305560. The old Hugo record wrongly reused the Schoen DOI.

### Newsletter and donations
- **Newsletter:** Brevo chosen (free: 300 emails/day, up to 100,000 contacts, fixed "Sent with Brevo" logo). Mailjet was considered and rejected (1,000 contact cap). Workflow: manual sends from the Brevo dashboard, chosen per post.
- Brevo form is embedded as an iframe (form host `65b8821f.sibforms.com`) in `_includes/subscribe.html`; the user updates the form in Brevo and pastes new embed code, so the URL may change. Current iframe height is 340 px, an estimate for the narrow margin column; adjust if scrollbars or gaps appear.
- **Gray background issue:** the gray around the white form box is Brevo's page body background (`#EFF2F7`), inside the cross-origin iframe, so site CSS cannot change it and the iframe height only crops it. Fix is in Brevo (Form design, Background section, set to white/transparent, then save/publish). As of last check the live form still served gray. Fallback: crop with an `overflow: hidden` wrapper.
- **Ko-fi:** username `benjamin_meyer` (https://ko-fi.com/benjamin_meyer), set in `_quarto.yml` footer and `_includes/kofi.html`.

### Open items
1. Brevo form: set background to white/transparent and publish, then tune the iframe height.
2. **Netlify:** `netlify.toml` on the branch is now Quarto. Still to do: check that Netlify branch deploys are not set to "All" (the branch is now on GitHub, so it may already be building), enable a branch deploy for `quarto-migration` only, and verify the preview builds. Optionally add `noindex` to the preview. Netlify is only an interim host (see Hosting plan).
3. **Redirects:** add `_redirects` or Netlify config; Hugo URLs for publications and events are not fully covered by `aliases:`.
4. **Cutover:** Hugo files are removed on the branch. Remaining: untrack `_site/` (tracked in git, shows as modified; add to `.gitignore`), decide on `LICENSE.md` (Wowchemy MIT license, kept for now), remove old Wowchemy notes from `To Do list`, then merge to `master` only when ready. The `.github/workflows/updater-wip.yml` Wowchemy updater was deleted on this branch.
5. **Content gaps:** finish the KDLL stub; delete the placeholder blog posts; add full author lists to publications (several show lead author plus "et al."); fill thin project and talk descriptions; unported drafts and AWC docs remain in `_archive/`. An unexplained file `projects/projects_list` appeared (open in the editor); check whether it is needed.
6. **Performance:** resize Skilak photos to about 1600 px wide; consider compressing publication PDFs.

### Hosting plan (decided Oct 31, 2026)
- Eventually cancel Netlify and publish the Quarto site directly from GitHub (repo `benjamin-meyer-ak/bmeyer_website`), preferably with a GitHub Actions workflow that runs `quarto render` and deploys `_site` to Pages (Pages source: "GitHub Actions"). Alternative: `quarto publish gh-pages`. Delete `netlify.toml` afterward.
- Without a custom domain the URL would be https://benjamin-meyer-ak.github.io/bmeyer_website/ (subpath breaks root-relative links unless `site-url` matches); renaming the repo to `benjamin-meyer-ak.github.io` removes the subpath.
- GitHub Pages has no `_redirects`; old Hugo URLs rely on `aliases:` (HTML meta-refresh), so cover publication and event URLs there. A custom domain needs DNS changes and a `CNAME` file in the output (add to `resources:`).
- Considering a new custom domain that is not the user's name: (a) short and content-based (e.g. coldrefuge, redd.fish, thermalsalmon) or (b) short and absurd (e.g. spruce-tip, balloonguy, thermalgoose). Undecided; plan to keep `benjamin-meyer.net` redirecting or for email/CV so old links survive. Not yet checked for availability or renewal price.

### Working notes
- Some files use Windows `\r\n` line endings, which can break `edit` string matches. Strip `\r` with `sed` first.

## Legacy Hugo Setup (live on `master` until cutover)

- **Theme:** Wowchemy v5, deprecated (superseded by Hugo Blox, which moved to Tailwind). Do not update or heavily customize it.
- **Hugo:** local 0.107.0 at `C:\Users\Benjamin\AppData\Roaming\Hugo\0.107.0\`; Netlify 0.108.0 pinned in `netlify.toml` (do not change). Go at `C:\Program Files\Go\bin\go.exe` for Hugo modules.
- **R packages:** `blogdown`, `rmarkdown`, `knitr`. `.Rprofile`: `blogdown.knit.on_save = TRUE`, `blogdown.method = 'html'`.
- **Workflow:** create `.Rmd` content, blogdown renders to `.html` on save, commit both, push to `master`, Netlify deploys. Hugo ignores `.Rmd` via `config.yaml` (intentional).
- **Local preview:** `options(blogdown.server.timeout = 120)` is required (Hugo module verification takes over 30 s), then `blogdown::serve_site()`. If `blogdown` is missing after an R upgrade, `install.packages("blogdown")`.
- **Config:** `config.yaml` (baseurl `https://benjamin-meyer.net/`, copyright still says 2025), `config/_default/menus.yaml`, `config/_default/params.yaml`, `go.mod` (Wowchemy pinned to a commit hash).
- **Content dirs (on `master` only):** `content/project/`, `content/post/`, `content/talk/`, `content/publication/`, `content/home/`, `static/`. 12 `.Rmd` files, each with an `.html` sibling.

## Planned Content (from To Do list)

**Research/professional posts:**
- Comment on Lamborn 2025 Chinook manuscript
- Comment on global hatcheries assessment
- Value of TIR-based conservation: engage with JM argument (minimal value because conserving cool inputs does not change overall warming trend; RAD approach preferred) vs. recent literature on measurable value of cool stepping stones
- Comment on the Ruffridge/Gillam debate (KDLL link in the To Do list)

**Project updates (current work first; cross-post to KWF blog):**
- Thermal imagery
- AWC expansion (1st priority; new tool from ADFG and collaborators)
- BOR project
- Vogel Lake
- CAP

**Personal posts:**
- Bus project
- Dog pics
- Research fraud talk (slides: docs.google.com/presentation/d/1Z76-IkH0aYePEnL7rS-gunI3yMhAR8DKuQ8okWjyCq0)

All blog posts are free and public.
