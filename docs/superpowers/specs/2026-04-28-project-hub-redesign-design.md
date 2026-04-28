# Minimal Project Hub Redesign

## Purpose

Convert the current al-folio academic template into a simple English project hub for promoting the owner's other websites and projects.

The site's main job is to provide clean, credible outbound links to other projects while supporting lightweight blog-based promotion and SEO. It should be easy to maintain and should not keep academic/demo sections that distract from the outbound-link purpose.

## Goals

- Present the owner's projects in a clear English portfolio-style hub.
- Give each project a stable internal page and a prominent outbound link.
- Support blog posts that promote projects, explain how to use them, and target useful search keywords.
- Keep the navigation and information architecture minimal.
- Remove or hide al-folio academic/demo content.

## Non-Goals

- Do not build a complex CMS.
- Do not preserve academic CV/publication/teaching workflows unless requested later.
- Do not create a heavy marketing site with many custom interactive sections.
- Do not require every project to have screenshots before launch.

## Recommended Approach

Use the existing Jekyll/al-folio structure, but simplify the visible site into a general project hub.

Keep:

- `_pages/about.md` for the homepage.
- `_pages/projects.md` for the main projects index.
- `_projects/*.md` for individual project pages.
- `_pages/blog.md` and `_posts/*.md` for promotional blog content.

Hide or remove from navigation:

- Publications
- CV
- Teaching
- People
- Repositories
- Dropdown/submenu demo page
- Demo/sample content that does not support the owner's projects

This is the fastest and lowest-risk route because it reuses the theme's project and blog collections instead of introducing a new system.

## Site Structure

Final navigation:

- `Home`
- `Projects`
- `Blog`
- `About`

### Home

The homepage should communicate the site's purpose quickly.

Recommended content:

- Short headline: a project hub for useful web projects, small games, tools, and experiments.
- One short paragraph explaining the site.
- Featured projects section with 3-6 project cards.
- Link to view all projects.
- Optional latest blog posts section.

### Projects

The projects page should be the main outbound-link directory.

Recommended content:

- Simple intro text.
- Project cards grouped by category or filterable later if needed.
- Each card should show:
  - Project name
  - Category
  - Short description
  - Status
  - Primary outbound link button, such as `Visit Project`
  - Optional screenshot or thumbnail

Initial categories:

- `Game`
- `Tool`
- `Content Site`
- `Web App`
- `Experiment`

These categories are intentionally broad so the site remains easy to maintain.

### Project Detail Pages

Every important external project should have an internal project page.

Required fields:

- Project name
- Category
- Short description
- External URL
- Status: `Live`, `Beta`, or `Archived`

Recommended page content:

- What the project does
- Who it is for
- Why it is useful or interesting
- Clear `Visit Project` outbound link
- Optional related blog links

### Blog

The blog should support promotion and SEO. It should stay simple and practical.

Recommended post types:

- Launch posts: announce a new project.
- How-to posts: explain how to use a project.
- Roundups: recommend several related projects or resources.
- Update posts: describe meaningful improvements.

Recommended categories:

- `launch`
- `guide`
- `roundup`
- `update`

### About

The About page should be short.

Recommended content:

- What this project hub is.
- What kinds of projects the owner builds.
- Optional contact or GitHub/social link.

## Content Cleanup

Remove, replace, or hide the current demo material:

- Default biography text
- Sample projects
- Sample blog posts
- Sample publications and bibliography entries
- Einstein CV data
- Sample teaching pages
- Sample book page
- Sample people/repository/dropdown pages

If deletion feels too destructive during the first pass, hide pages from navigation first and replace visible content with real placeholder project-hub content.

## Data Model

Continue using `_projects/*.md`, with frontmatter extended for project-hub needs.

Recommended project frontmatter:

```yaml
---
layout: page
title: Project Name
description: One-sentence project summary.
category: Tool
status: Live
external_url: https://example.com
img: /assets/img/project-image.jpg
importance: 1
---
```

`external_url` may require a small template update so project pages and cards can display a consistent outbound button.

Recommended blog frontmatter:

```yaml
---
layout: post
title: Post Title
date: YYYY-MM-DD
categories: launch
description: One-sentence summary.
related_project: project-slug
---
```

`related_project` is optional. It can be used later to connect posts to project pages.

## SEO And Linking Requirements

- Every project should have a stable internal page.
- Every project page should include at least one prominent outbound link.
- Blog posts should link to the relevant internal project page and, where appropriate, the external project URL.
- Page titles and descriptions should be plain English and keyword-aware without sounding spammy.
- Keep outbound links natural and useful.

## Implementation Notes

- Update `_config.yml` for English project-hub metadata.
- Set `lang: en`.
- Keep `url: https://quotuo.github.io`.
- Recheck whether `baseurl` should be empty for this root GitHub Pages site.
- Change page navigation through `_pages/*.md` frontmatter.
- Prefer editing existing al-folio layouts/includes before adding new ones.
- If project cards need outbound buttons, update the relevant project include/template rather than duplicating markup in every project file.

## Verification

Minimum verification after implementation:

- Run formatting on changed files if dependencies are installed.
- Run the site locally with Docker: `docker compose up`.
- Open `http://localhost:8080`.
- Check:
  - Navigation only shows the intended pages.
  - Home, Projects, Blog, and About render correctly.
  - Project cards show project metadata and outbound links.
  - Blog posts render and link to project pages where applicable.
  - No obvious demo academic content remains visible.

