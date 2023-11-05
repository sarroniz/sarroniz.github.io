---
layout: page
permalink: /teaching/
title: teaching
description: materials for courses I currently teach or taught in the past
nav: true
nav_order: 5
display_categories: [work, fun]
horizontal: false
---

<!-- pages/projects.md -->
<div class="projects">
{%- if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {%- for category in page.display_categories %}
  <h2 class="category">{{ category }}</h2>
  {%- assign categorized_projects = site.projects | where: "category", category -%}
  {%- assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
  {% endfor %}

{%- else -%}
<!-- Display projects without categories -->
  {%- assign sorted_projects = site.projects | sort: "importance" -%}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
{%- endif -%}
</div>

---
 
**Teaching assignments:**

- HISP-S 150: _Elementary Spanish II_ (Fall 2016)

- HISP-S 200: _Second-year Spanish I_ (Fall 2017; Spring 2018)

- HISP-S 250: _Second-year Spanish II_ (Spring 2017; Fall 2019)

- HISP-S 280: _Spanish Grammar in Context_ (Spring 2020 - second half online)

- HISP-S 326: _Introduction to Hispanic Linguistics_ (Fall 2020 - hybrid; Spring 2021 - online)

- HISP-S 315: _Spanish in the Business World_ (Fall 2021)

- HISP-S 281: _Spanish Grammar in Context for Heritage Speakers_ (Spring 2022)

- HISP-S 308: _Composition and Conversation in Spanish_ (Fall 2022, Spring 2023)