# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal academic website for Logan Mondal Bhamidipaty (AI PhD student, University of Edinburgh). Jekyll static site deployed to GitHub Pages at `https://flyingworkshop.github.io/logan-mondal-bhamidipaty`.

There is no test suite, linter, or build script beyond Jekyll itself.

## Commands

```bash
bundle install                          # first-time setup
bundle exec jekyll serve --livereload   # local dev server
```

`_config.yml` is **not** hot-reloaded — restart the server after editing it.

Deployment is fully automatic: `.github/workflows/jekyll.yml` builds with `actions/jekyll-build-pages@v1` and deploys to GitHub Pages on every push to `main`. Adding a file to `_posts/` and pushing is the entire publish flow. `_site/` is gitignored and must never be committed; the copy on disk is a stale local `jekyll serve` artifact.

## Architecture

The site renders as a **single page**. `index.html` is a 3-line stub declaring `layout: home`. `_layouts/home.html` is the only layout and composes four includes in fixed order:

```
bio.html → section.html(category="research") → section.html(category="teaching") → footer.html
```

`_includes/section.html` is the heart of the site: it loops `site.posts`, filters on `post.categories contains include.category`, and renders each as a two-cell table row (thumbnail | title/authors/venue/links/excerpt). **Posts never render as standalone pages** — there is no `post` layout, and `_config.yml` sets `permalink: /`. A post is a publication card, not an article.

The two category names are hardcoded in `_layouts/home.html`. A post with any `category` other than `research` or `teaching` silently renders nowhere.

## Adding a publication

Create `_posts/YYYY-MM-DD-slug.md`. Representative example with the full set of commonly used keys:

```yaml
---
title:  "DynaDojo: An Extensible Platform for Benchmarking Scaling in Dynamical System Identification"
date:   2023-12-15 11:54:37 -0800
authors: <strong>Logan Mondal Bhamidipaty*</strong>, Tommy Bruzzese*, Caryn Tran*, Rami Mrad, Max S. Kanwal
category: research
image: hot_dino.gif
venue: Conference on Neural Information Processing Systems (NeurIPS)
paper: https://proceedings.neurips.cc/paper_files/...
code: https://github.com/DynaDojo/dynadojo
poster: assets/images/dynadojo-poster.png
---
One paragraph of abstract-style prose.
```

Required: `title`, `date`, `authors`, `category`, `image`. Optional link keys: `paper`, `video`, `code`, `poster`, `slides`, `website`, `youtube`. There is no `layout` key and no dedicated arXiv key — **arXiv links go in `paper:`**.

`authors` is a raw HTML string; Logan's own name is always wrapped in `<strong>`, with `*` for equal contribution placed *inside* the tag. Link render order is fixed by `section.html`, not by frontmatter order.

### Non-obvious rules

- **`date:` controls ordering, not the filename.** Posts sort newest-first by the frontmatter `date`, which overrides the filename prefix — and this repo deliberately relies on that (e.g. `2024-07-18-compressed.md` carries `date: 2025-06-20` to sit at the top). Every post uses the same literal time `11:54:37 -0800`. Never date a post in the future: GitHub Pages excludes future-dated posts, and the entry will vanish from the deployed site.
- **Keep the body to one paragraph.** `section.html` renders `{{ post.excerpt }}`, and no `excerpt_separator` is configured, so Jekyll's default splits on the first blank line and everything after it is dropped.
- **`image:` is a bare filename**, not a path. `section.html` hardcodes the prefix `./assets/images/`. Some existing posts write a leading slash (`image: /maze.png`), producing a harmless double slash — prefer the no-slash form. `poster:`, by contrast, takes the full site-relative path `assets/images/foo.png`.
- **Do not use `{{ site.baseurl }}` in post paths.** `_config.yml` sets `baseurl: "/logan-mondal-bhamidipaty"`, but every template uses document-relative `./assets/...` and ignores it. Introducing baseurl would double the prefix.
- Thumbnails render at ~120–200px in a 25%-width cell. Aim for square-ish, ≥400px on the short side. GIFs are treated identically to PNGs.

## Other content

- **Bio text** — `_includes/bio.html`, two `<p>` blocks (factual/affiliational, then an italicized research-interests statement). Collaborator names are raw `<a href>` links. Prose is first person with contractions.
- **Contact links** — `_data/contacts.yml` (`title`/`url` pairs), rendered by `_includes/contacts.html`. Add links here, not in `bio.html`. The CV lives at `assets/pdfs/logan_bhamidipaty_short_cv.pdf` and is referenced only from this file, so replacing the CV in place needs no code change.
- **Styles** — `assets/css/styles.scss`. Layout is table-based throughout, capped at `max-width:800px`.

## Stale files — do not trust

`notes.md` documents a post format (`layout: post`, `subtitle:`, `author:`, plural `categories:`) that matches **no** actual post, and an image rule ("160 x 160") that nothing in the repo follows. Trust `_posts/` and `_includes/section.html` instead. `_includes/footer.html` hardcodes "Last updated October 2024."
