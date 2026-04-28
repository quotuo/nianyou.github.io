---
layout: about
title: home
permalink: /
subtitle: Useful web projects, small games, tools, and experiments.
nav: false
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
