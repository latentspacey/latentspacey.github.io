# Latent Space

Personal blog of [@latentspacey](https://github.com/latentspacey) — notes on AI, machine learning, and adjacent ideas.

Live at **https://latentspacey.github.io**

Built with [Astro](https://astro.build/) and the [AstroPaper](https://github.com/satnaing/astro-paper) theme. Deploys automatically to GitHub Pages on every push to `main`.

## Local development

```bash
npm install
npm run dev          # http://localhost:4321
npm run build        # produce static site in ./dist
npm run preview      # serve the built site
```

## Adding a post

Drop a markdown file into `src/data/blog/` with this frontmatter:

```yaml
---
author: Rahul Kumar
pubDatetime: 2026-05-08T10:00:00Z
title: Your post title
slug: your-post-slug
featured: false
draft: false
tags:
  - tag
description: One-line summary.
---
```

For images, put files in `public/` (e.g. `public/posts/loss-curve.png`) and reference them as `/posts/loss-curve.png` in markdown.

## Project structure

```
src/
├── config.ts           # site-wide config (title, author, URL)
├── constants.ts        # social links
├── data/blog/          # ← write your posts here
├── pages/about.md      # the /about page
└── layouts/            # post and page layouts
public/                 # static files (images, favicon, OG)
.github/workflows/      # GitHub Actions deploy
```

## Credits

Theme by [Sat Naing](https://github.com/satnaing) — released under the MIT license.
