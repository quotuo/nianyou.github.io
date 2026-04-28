# Project Hub Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Convert the current al-folio academic demo site into a minimal English project hub with Home, Projects, Blog, and About.

**Architecture:** Reuse Jekyll collections already present in al-folio. `_projects/*.md` remains the source of project pages, `_posts/*.md` remains the source of blog posts, and small Liquid include updates add project-hub metadata and outbound links. Navigation is controlled through `_pages/*.md` frontmatter.

**Tech Stack:** Jekyll, Liquid, Markdown frontmatter, SCSS/CSS already bundled by al-folio, Docker Compose for local verification, Prettier with `@shopify/prettier-plugin-liquid` when dependencies are installed.

---

## File Structure

- Modify `_config.yml`: site metadata, blog labels, displayed blog categories/tags, and `baseurl`.
- Modify `_pages/about.md`: convert root page into `Home`.
- Create `_pages/about-site.md`: short standalone About page.
- Modify `_pages/projects.md`: convert from demo academic projects list into project directory.
- Modify `_pages/blog.md`: adjust title/order for promotional blog.
- Modify `_pages/publications.md`, `_pages/cv.md`, `_pages/teaching.md`, `_pages/profiles.md`, `_pages/repositories.md`, `_pages/dropdown.md`, `_pages/books.md`, `_pages/news.md`: hide from navigation.
- Modify `_includes/projects.liquid`: add category/status metadata and explicit outbound button.
- Modify `_includes/projects_horizontal.liquid`: keep behavior consistent if horizontal project cards are enabled later.
- Replace `_projects/*.md`: remove demo project content and create one neutral project-intake entry until real project URLs are provided.
- Replace most `_posts/*.md`: remove demo al-folio posts and add starter promotional blog posts.
- Optionally update `CODEX_PROJECT_CONTEXT.md`: note that the site has been simplified into a project hub.

## Task 1: Simplify Global Site Settings And Navigation

**Files:**

- Modify: `_config.yml`
- Modify: `_pages/blog.md`
- Modify: `_pages/publications.md`
- Modify: `_pages/cv.md`
- Modify: `_pages/teaching.md`
- Modify: `_pages/profiles.md`
- Modify: `_pages/repositories.md`
- Modify: `_pages/dropdown.md`
- Modify: `_pages/books.md`
- Modify: `_pages/news.md`

- [ ] **Step 1: Inspect current nav and config**

Run:

```bash
rg -n "^(title|first_name|middle_name|last_name|description|keywords|lang|url|baseurl|blog_name|blog_description|display_tags|display_categories|enable_project_categories):|^(title|permalink|nav|nav_order|dropdown):" _config.yml _pages
```

Expected: output shows default al-folio identity and nav pages for blog, publications, projects, repositories, CV, teaching, people, and submenus.

- [ ] **Step 2: Update `_config.yml` identity and blog labels**

Edit `_config.yml` so the key values read:

```yaml
title: QuoTuo Project Hub
first_name:
middle_name:
last_name:
contact_note: >
  Building and curating useful web projects, tools, games, and experiments.
description: >
  A minimal project hub for web tools, small games, content sites, and experiments created by QuoTuo.
footer_text: >
  Built with <a href="https://jekyllrb.com/" target="_blank">Jekyll</a> and hosted on <a href="https://pages.github.com/" target="_blank">GitHub Pages</a>.
keywords: web projects, indie projects, online tools, browser games, project hub
lang: en
icon: Q

url: https://quotuo.github.io
baseurl:
```

Also update the blog display settings:

```yaml
blog_name: Project Notes
blog_description: Launch notes, guides, updates, and project roundups.
display_tags: ["projects", "tools", "games", "seo"]
display_categories: ["launch", "guide", "roundup", "update"]
enable_project_categories: true
```

- [ ] **Step 3: Hide academic/demo pages from navigation**

Set `nav: false` in these files:

```yaml
# _pages/publications.md
nav: false

# _pages/cv.md
nav: false

# _pages/teaching.md
nav: false

# _pages/profiles.md
nav: false

# _pages/repositories.md
nav: false

# _pages/dropdown.md
nav: false

# _pages/books.md
nav: false

# _pages/news.md
nav: false
```

Keep the files in place during the first implementation pass to reduce routing risk.

- [ ] **Step 4: Update Blog nav order**

In `_pages/blog.md`, keep the permalink and pagination, but set:

```yaml
title: blog
nav: true
nav_order: 3
```

- [ ] **Step 5: Verify nav frontmatter**

Run:

```bash
rg -n "^(title|permalink|nav|nav_order|dropdown):" _pages
```

Expected: only the intended pages have `nav: true` after later tasks finish. At this point, `projects` and `blog` should be visible, and hidden pages should show `nav: false`.

- [ ] **Step 6: Commit Task 1**

Run:

```bash
git add _config.yml _pages/blog.md _pages/publications.md _pages/cv.md _pages/teaching.md _pages/profiles.md _pages/repositories.md _pages/dropdown.md _pages/books.md _pages/news.md
git commit -m "config: Simplify project hub navigation"
```

## Task 2: Build The Home And About Pages

**Files:**

- Modify: `_pages/about.md`
- Create: `_pages/about-site.md`

- [ ] **Step 1: Replace `_pages/about.md` with project hub homepage**

Use this content:

```markdown
---
layout: about
title: home
permalink: /
subtitle: Useful web projects, small games, tools, and experiments.
nav: true
nav_order: 1

profile:
  align: right
  image:
  image_circular: false
  more_info:

selected_papers: false
social: false

announcements:
  enabled: false
  scrollable: false
  limit: 0

latest_posts:
  enabled: true
  scrollable: false
  limit: 3
---

QuoTuo Project Hub collects practical web projects, lightweight games, tools, content sites, and experiments in one place.

Each project has a short internal page for context and a clear outbound link to the live site. The blog shares launch notes, guides, roundups, and updates that support discovery for those projects.

## Featured Projects

<div class="projects">
  <div class="row row-cols-1 row-cols-md-3">
    {% assign featured_projects = site.projects | where: "featured", true | sort: "importance" %}
    {% for project in featured_projects limit: 6 %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>

[View all projects]({{ '/projects/' | relative_url }})
```

- [ ] **Step 2: Create `_pages/about-site.md`**

Use this content:

```markdown
---
layout: page
title: about
permalink: /about/
description: About QuoTuo Project Hub.
nav: true
nav_order: 4
---

QuoTuo Project Hub is a simple home for independent web projects. It highlights useful tools, small browser games, content sites, and experiments from one place.

The goal is practical: make each project easier to discover, explain what it does, and provide a clear path to visit the live site.
```

- [ ] **Step 3: Verify Home and About page frontmatter**

Run:

```bash
rg -n "^(layout|title|permalink|nav|nav_order|selected_papers|social):" _pages/about.md _pages/about-site.md
```

Expected: `_pages/about.md` has `title: home`, `permalink: /`, `nav: true`, `nav_order: 1`, `selected_papers: false`, and `social: false`. `_pages/about-site.md` has `permalink: /about/`, `nav: true`, and `nav_order: 4`.

- [ ] **Step 4: Commit Task 2**

Run:

```bash
git add _pages/about.md _pages/about-site.md
git commit -m "feat: Add project hub home and about pages"
```

## Task 3: Convert Project Cards To Project-Hub Cards

**Files:**

- Modify: `_includes/projects.liquid`
- Modify: `_includes/projects_horizontal.liquid`

- [ ] **Step 1: Replace `_includes/projects.liquid`**

Use this content:

```liquid
<div class="col">
  <div class="card h-100 hoverable">
    <a href="{{ project.url | relative_url }}" aria-label="Read more about {{ project.title }}">
      {% if project.img %}
        {%
          include figure.liquid
          loading="eager"
          path=project.img
          sizes = "250px"
          alt=project.title
          class="card-img-top"
        %}
      {% endif %}
    </a>
    <div class="card-body d-flex flex-column">
      <div class="mb-2">
        {% if project.category %}
          <span class="badge badge-light text-uppercase">{{ project.category }}</span>
        {% endif %}
        {% if project.status %}
          <span class="badge badge-secondary">{{ project.status }}</span>
        {% endif %}
      </div>
      <h2 class="card-title">
        <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
      </h2>
      <p class="card-text">{{ project.description }}</p>
      <div class="mt-auto">
        {% if project.external_url %}
          <a class="btn btn-sm btn-primary" href="{{ project.external_url }}" target="_blank" rel="noopener noreferrer">Visit Project</a>
        {% endif %}
        <a class="btn btn-sm btn-outline-secondary" href="{{ project.url | relative_url }}">Details</a>
      </div>
    </div>
  </div>
</div>
```

- [ ] **Step 2: Replace `_includes/projects_horizontal.liquid`**

Use this content:

```liquid
<div class="col mb-4">
  <div class="card h-100 hoverable">
    <div class="row no-gutters">
      {% if project.img %}
        <div class="col-md-6">
          <a href="{{ project.url | relative_url }}" aria-label="Read more about {{ project.title }}">
            {% include figure.liquid loading="eager" path=project.img sizes="(min-width: 768px) 156px, 50vw" alt=project.title class="card-img" %}
          </a>
        </div>
      {% endif %}
      <div class="{% if project.img %}col-md-6{% else %}col-md-12{% endif %}">
        <div class="card-body d-flex flex-column h-100">
          <div class="mb-2">
            {% if project.category %}
              <span class="badge badge-light text-uppercase">{{ project.category }}</span>
            {% endif %}
            {% if project.status %}
              <span class="badge badge-secondary">{{ project.status }}</span>
            {% endif %}
          </div>
          <h3 class="card-title">
            <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
          </h3>
          <p class="card-text">{{ project.description }}</p>
          <div class="mt-auto">
            {% if project.external_url %}
              <a class="btn btn-sm btn-primary" href="{{ project.external_url }}" target="_blank" rel="noopener noreferrer">Visit Project</a>
            {% endif %}
            <a class="btn btn-sm btn-outline-secondary" href="{{ project.url | relative_url }}">Details</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Verify card templates contain outbound link support**

Run:

```bash
rg -n "external_url|Visit Project|badge|Details" _includes/projects.liquid _includes/projects_horizontal.liquid
```

Expected: both include files contain `external_url`, `Visit Project`, `badge`, and `Details`.

- [ ] **Step 4: Commit Task 3**

Run:

```bash
git add _includes/projects.liquid _includes/projects_horizontal.liquid
git commit -m "feat: Add outbound project card links"
```

## Task 4: Replace Demo Projects With A Neutral Starter Entry

**Files:**

- Modify: `_pages/projects.md`
- Delete: `_projects/1_project.md` through `_projects/9_project.md`
- Create: `_projects/project-directory.md`

- [ ] **Step 1: Replace `_pages/projects.md`**

Use this content:

```markdown
---
layout: page
title: projects
permalink: /projects/
description: A directory of live projects, tools, games, content sites, and experiments.
nav: true
nav_order: 2
display_categories: [Game, Tool, Content Site, Web App, Experiment]
horizontal: false
---

<!-- pages/projects.md -->

This page collects active projects with short descriptions and direct links to the live sites.

<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  {% for category in page.display_categories %}
    {% assign categorized_projects = site.projects | where: "category", category %}
    {% if categorized_projects.size > 0 %}
      <a id="{{ category | slugify }}" href=".#{{ category | slugify }}">
        <h2 class="category">{{ category }}</h2>
      </a>
      {% assign sorted_projects = categorized_projects | sort: "importance" %}
      <div class="row row-cols-1 row-cols-md-3">
        {% for project in sorted_projects %}
          {% include projects.liquid %}
        {% endfor %}
      </div>
    {% endif %}
  {% endfor %}
{% else %}
  {% assign sorted_projects = site.projects | sort: "importance" %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
{% endif %}
</div>
```

- [ ] **Step 2: Remove demo project files**

Run:

```bash
rm _projects/1_project.md _projects/2_project.md _projects/3_project.md _projects/4_project.md _projects/5_project.md _projects/6_project.md _projects/7_project.md _projects/8_project.md _projects/9_project.md
```

Expected: the nine al-folio demo project files are removed.

- [ ] **Step 3: Create `_projects/project-directory.md`**

Use this content:

```markdown
---
layout: page
title: Project Directory
description: A living directory for live projects, tools, games, content sites, and experiments.
category: Web App
status: Live
external_url: https://quotuo.github.io
featured: true
importance: 1
---

Project Directory is the starting point for QuoTuo Project Hub.

As real project links are added, this page can be replaced by individual project pages for each live website.

[Visit Project](https://quotuo.github.io){:target="_blank" rel="noopener noreferrer" .btn .btn-primary}
```

- [ ] **Step 4: Verify project metadata**

Run:

```bash
rg -n "^(title|description|category|status|external_url|featured|importance):" _projects
```

Expected: the starter project has `category`, `status`, `external_url`, `featured`, and `importance`.

- [ ] **Step 5: Commit Task 4**

Run:

```bash
git add _pages/projects.md _projects
git commit -m "feat: Replace demo projects with project hub entry"
```

## Task 5: Replace Demo Blog Content With Starter Promotional Posts

**Files:**

- Delete: current `_posts/*.md` demo files
- Create: `_posts/2026-04-28-welcome-to-quotuo-project-hub.md`
- Create: `_posts/2026-04-28-how-this-project-hub-is-organized.md`

- [ ] **Step 1: Remove demo post files**

Run:

```bash
rm _posts/*.md
```

Expected: all al-folio demo blog posts are removed.

- [ ] **Step 2: Create `_posts/2026-04-28-welcome-to-quotuo-project-hub.md`**

Use this content:

```markdown
---
layout: post
title: Welcome to QuoTuo Project Hub
date: 2026-04-28
categories: launch
tags: [projects]
description: A short introduction to this project hub and the kinds of projects it will collect.
---

QuoTuo Project Hub is a simple place to collect useful web projects, lightweight games, tools, content sites, and experiments.

The goal is practical: each project gets a clear explanation, a stable internal page, and a direct link to the live site. New posts will highlight launches, updates, guides, and roundups for projects published here.

[Explore the projects]({{ '/projects/' | relative_url }})
```

- [ ] **Step 3: Create `_posts/2026-04-28-how-this-project-hub-is-organized.md`**

Use this content:

```markdown
---
layout: post
title: How This Project Hub Is Organized
date: 2026-04-28
categories: guide
tags: [projects, tools, games]
description: How projects are grouped and how blog posts support project discovery.
---

Projects are grouped into broad categories such as games, tools, content sites, web apps, and experiments.

Project pages provide the stable reference point. Blog posts add context: launch notes explain what is new, guides show how to use a project, roundups connect related resources, and updates document meaningful improvements.

[Browse all projects]({{ '/projects/' | relative_url }})
```

- [ ] **Step 4: Verify blog metadata**

Run:

```bash
rg -n "^(title|date|categories|tags|description):" _posts
```

Expected: both starter posts use `launch` or `guide` categories and contain concise descriptions.

- [ ] **Step 5: Commit Task 5**

Run:

```bash
git add _posts
git commit -m "feat: Add starter project hub blog posts"
```

## Task 6: Update Local Codex Context

**Files:**

- Modify: `CODEX_PROJECT_CONTEXT.md`

- [ ] **Step 1: Update the current-state section**

Replace the `Current State` section with:

```markdown
## Current State

This repo has been simplified from the default al-folio academic demo into a minimal English project hub.

- Visible navigation should be limited to `Home`, `Projects`, `Blog`, and `About`.
- `_projects/*.md` stores project entries with `category`, `status`, `external_url`, `featured`, and `importance` frontmatter.
- `_posts/*.md` stores promotional blog content such as launch notes, guides, roundups, and updates.
- Academic/demo sections such as publications, CV, teaching, people, repositories, and dropdown demos should remain hidden unless the user explicitly asks to restore them.
- Project cards should provide both an internal `Details` link and an outbound `Visit Project` link.
```

- [ ] **Step 2: Update likely customization guidance**

Replace the `Likely First Customization Pass` section with:

```markdown
## Likely Next Customization Pass

When the user provides real project information:

1. Replace starter `_projects/*.md` entries with real project names, categories, descriptions, and `external_url` values.
2. Add real screenshots to `assets/img/` and reference them with `img: /assets/img/name.jpg`.
3. Write one launch blog post per important project.
4. Add guide or roundup posts only when they help promote a project naturally.
5. Keep navigation minimal unless a new page has a clear promotional purpose.
```

- [ ] **Step 3: Verify context mentions project hub**

Run:

```bash
rg -n "project hub|Visit Project|external_url|Home.*Projects.*Blog.*About" CODEX_PROJECT_CONTEXT.md
```

Expected: context describes the simplified project hub model.

- [ ] **Step 4: Commit Task 6**

Run:

```bash
git add CODEX_PROJECT_CONTEXT.md
git commit -m "docs: Update Codex project hub context"
```

## Task 7: Full Verification

**Files:**

- Read-only verification across changed files.

- [ ] **Step 1: Check visible nav state**

Run:

```bash
rg -n "nav: true|nav: false" _pages
```

Expected: `nav: true` appears only on Home, Projects, Blog, and About. Other preserved pages should show `nav: false` or no visible nav entry.

- [ ] **Step 2: Check no obvious demo content remains visible**

Run:

```bash
rg -n "Einstein|al-folio|selected publications|publications by categories|teaching|submenus|torvalds|alshedivat" _pages _projects _posts _data _config.yml
```

Expected: no matches in visible pages, project entries, or starter posts. Matches may remain in hidden data files only if they are not rendered.

- [ ] **Step 3: Run formatting check if dependencies are available**

Run:

```bash
npx prettier _config.yml _pages/about.md _pages/about-site.md _pages/projects.md _pages/blog.md _includes/projects.liquid _includes/projects_horizontal.liquid _projects/*.md _posts/*.md CODEX_PROJECT_CONTEXT.md --check
```

Expected: pass. If it fails because `@shopify/prettier-plugin-liquid` is missing, either install dependencies with user approval or record that formatting could not be verified.

- [ ] **Step 4: Run local site with Docker**

Run:

```bash
docker compose up
```

Expected: Jekyll server starts and reports a local server at `http://0.0.0.0:8080` or equivalent.

- [ ] **Step 5: Browser-check the site**

Open `http://localhost:8080` and verify:

- Home renders.
- Navigation shows Home, Projects, Blog, About.
- Featured project cards show category/status badges.
- `Visit Project` buttons are visible.
- `/projects/` renders grouped project sections.
- `/blog/` renders the two starter posts.
- `/about/` renders the short About page.

- [ ] **Step 6: Commit verification-only fixes if needed**

If verification required small fixes, run:

```bash
git add <fixed-files>
git commit -m "fix: Polish project hub verification issues"
```

If no fixes were needed, do not create an empty commit.
