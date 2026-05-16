# SRE Public Notes

Public SRE knowledge base built with [Just the Docs](https://just-the-docs.com/) and GitHub Pages.

## Local preview

Use Ruby 3.3.x.

```bash
bundle install
bundle exec jekyll serve
```

The site will be available at <http://localhost:4000>.

## Publish on GitHub Pages

1. Create a public GitHub repository.
2. Push this repository to `main`.
3. In GitHub, open Settings -> Pages.
4. Set Build and deployment -> Source to GitHub Actions.
5. The `Deploy Jekyll site to Pages` workflow will publish the site.

## Add pages

Create Markdown files under `docs/`. For a page inside the SRE section, use front matter like this:

```yaml
---
title: Page Title
layout: default
parent: SRE
nav_order: 60
---
```
