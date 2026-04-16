# koiralasumann.github.io

Personal site and blog for Suman Koirala. Built with Astro + Tailwind CSS, deployed to GitHub Pages.

## Local development

```bash
npm install
npm run dev
```

Site runs at `http://localhost:4321/`.

## Creating a new blog post

1. Create a file at `src/content/blog/<slug>.md` (or `.mdx`)
2. Add frontmatter:

```yaml
---
title: "Your post title"
description: "One sentence for search results. Under 160 chars."
pubDate: 2024-12-15
tags: ["zapier", "api"]
draft: true
---
```

3. Write your post below the frontmatter
4. Posts with `draft: true` only show up in `npm run dev`, not in production builds

## Publishing a post

Set `draft: false` in the frontmatter, commit, and push to `main`. GitHub Actions builds and deploys automatically.

## Building for production

```bash
npm run build
```

Output goes to `dist/`. The GitHub Actions workflow handles this on push to `main`.

## Project structure

```
src/
  content/blog/    # Blog posts (.md / .mdx)
  components/      # Reusable Astro components
  layouts/         # Page layouts
  pages/           # Route pages
  styles/          # Global CSS
```
