---
layout: default
title: Projects
---
<h1 class="projects"> {{ page.title }}</h1>

Stuff I'm working on, and stuff I've shipped in the past. Most of these are open source &mdash; feel free to check out the code.

{% assign projects = site.projects | sort: "order" | reverse %}
{% for project in projects %}
  {% include menu_link.html title=project.title url=project.url %}
{% endfor %}
