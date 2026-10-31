# bmeyer_website

Personal research website for Benjamin Meyer, built with [Quarto](https://quarto.org/) and deployed on Netlify.

- `index.qmd`: landing page
- `blog/`, `projects/research/`, `projects/other/`, `talks/`, `publications/`: one folder per item, each with an `index.qmd`
- `_quarto.yml`: navbar, footer, theme
- `uploads/`, `images/`: static files
- `_includes/`: Brevo subscribe form and Ko-fi button
- `_archive/`: leftover Hugo/Wowchemy material not ported to the Quarto site

Preview with `quarto preview`; build with `quarto render` (output in `_site`).
