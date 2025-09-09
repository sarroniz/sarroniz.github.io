---
layout: page
title: teaching
permalink: /teaching/
description: materials for courses I currently teach or taught in the past
nav: true
nav_order: 5
display_categories: [UNR, IU, workshops, other]
horizontal: false
---

<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_teaching = site.teaching | where: "category", category %}
  {% assign sorted_teaching = categorized_teaching | sort: "importance" %}

  {% if page.horizontal %}
    <div class="container">
      <div class="row row-cols-2">
      {% for item in sorted_teaching %}
        {% include projects_horizontal.html project=item %}
      {% endfor %}
      </div>
    </div>
  {% else %}
    <div class="grid">
      {% for item in sorted_teaching %}
        {% include teaching_card.html item=item %}
      {% endfor %}
    </div>
  {% endif %}
  {% endfor %}
{% endif %}
</div>