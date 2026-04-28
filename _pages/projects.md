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
