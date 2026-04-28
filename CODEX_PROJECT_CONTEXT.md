# Codex Project Context

Read this before making future changes in this repository.

## Project Identity

- Repository: `quotuo/quotuo.github.io`
- Site type: minimal English project hub for promoting the owner's other websites and projects.
- Primary purpose: provide credible internal project pages, promotional blog posts, and clear outbound links to live projects.
- Stack: Jekyll, Liquid, Markdown, SCSS, Docker Compose, GitHub Pages.
- Recommended local server: `docker compose up`, then open `http://localhost:8080`.

## Current State

This repo has been simplified from the default al-folio academic demo into a minimal English project hub.

- Visible navigation should be limited to `Home`, `Projects`, `Blog`, and `About`.
- `_projects/*.md` stores project entries with `category`, `status`, `external_url`, `featured`, and `importance` frontmatter.
- `_posts/*.md` stores promotional blog content such as launch notes, guides, roundups, and updates.
- Academic/demo sections such as publications, CV, teaching, people, repositories, and dropdown demos should remain hidden unless the user explicitly asks to restore them.
- Project cards should provide both an internal `Details` link and an outbound `Visit Project` link.

## Where To Change Common Requests

- Site metadata, blog labels, search tags/categories, URL/baseurl: `_config.yml`
- Home page content and featured project block: `_pages/about.md`
- About page: `_pages/about-site.md`
- Project directory page: `_pages/projects.md`
- Blog index: `_pages/blog.md`
- Project card markup: `_includes/projects.liquid` and `_includes/projects_horizontal.liquid`
- Individual project pages: `_projects/*.md`
- Promotional blog posts: `_posts/YYYY-MM-DD-title.md`
- Static images and downloads: `assets/img/`, `assets/pdf/`, `assets/video/`, `assets/audio/`

## Project Entry Pattern

Use this frontmatter for each real project:

```yaml
---
layout: page
title: Project Name
description: One-sentence project summary.
category: Tool
status: Live
external_url: https://project-url.com
featured: true
importance: 1
---
```

Valid broad categories:

- `Game`
- `Tool`
- `Content Site`
- `Web App`
- `Experiment`

Valid status values:

- `Live`
- `Beta`
- `Archived`

## Blog Pattern

Use promotional posts for launches, guides, roundups, and updates.

```yaml
---
layout: post
title: Post Title
date: YYYY-MM-DD
categories: launch
tags: [projects]
description: One-sentence summary.
---
```

Recommended categories:

- `launch`
- `guide`
- `roundup`
- `update`

## URL And Deployment Notes

This is a root GitHub Pages repository named `quotuo.github.io`.

- Keep `url: https://quotuo.github.io`.
- Keep `baseurl:` empty for the root site unless testing proves otherwise.
- If changing `url` or `baseurl`, test CSS/JS paths locally and after deployment.

## Likely Next Customization Pass

When the user provides real project information:

1. Replace starter `_projects/*.md` entries with real project names, categories, descriptions, and `external_url` values.
2. Add real screenshots to `assets/img/` and reference them with `img: /assets/img/name.jpg`.
3. Write one launch blog post per important project.
4. Add guide or roundup posts only when they help promote a project naturally.
5. Keep navigation minimal unless a new page has a clear promotional purpose.

## Verification

Use verification proportional to the change:

- Content-only edits: check frontmatter and run Prettier if dependencies are installed.
- Config, Liquid, or layout edits: run `docker compose up` and inspect `http://localhost:8080`.
- Check that visible navigation remains `Home`, `Projects`, `Blog`, and `About`.
- Check that project cards show `Visit Project` and `Details`.

