# Coursemojo Engineering Blog

Our engineering blog. Published via GitHub Pages with Jekyll.

## Adding a new post

Create a markdown file in `_posts/` with the naming convention `YYYY-MM-DD-title-slug.md` and add front matter:

```yaml
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
author: Your Name
description: "A short description for SEO (under 160 chars)."
---
```

Push to `main` and GitHub Pages builds and deploys automatically.

## Local preview

```bash
bundle install
bundle exec jekyll serve
```

Then visit `http://localhost:4000/engineering-blog/`.
