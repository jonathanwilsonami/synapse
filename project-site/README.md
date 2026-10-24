# Project Site Overview

The SYNAPSE project site is built with **Quarto**, which converts `.qmd` files into a static website. `_quarto.yml` defines the navbar, theme, and output folder (`docs/`, which GitHub Pages serves).

## Pages

- **Home** → `index.qmd`
- **About** → `about.qmd`
- **Paper** → `paper.qmd`

## Other folders

- `images/` — images used by the site
- `assets/` — generated figures/tables for the paper (empty for now)
- `docs/` — rendered site output (`quarto render`); committed and published by the GitHub workflow

## Working on the site

Install Quarto: <https://quarto.org/docs/get-started/>

```bash
quarto preview   # live local preview
quarto render    # render the whole site into docs/
```

## Publishing

`.github/workflows/gh-pages.yml` pushes the committed `project-site/docs/` folder to the `gh-pages` branch on every push to `main`. It does not build the site, so run `quarto render` before pushing.

Manual publish:

```bash
quarto publish gh-pages
```
