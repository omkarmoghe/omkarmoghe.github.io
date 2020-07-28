---
layout: default
title: Projects
---
<h1 class="projects"> {{ page.title }}</h1>

Cool stuff I'm working on.

{% assign projects = site.projects | sort: "order" | reverse %}
{% for project in projects %}
  {% include menu_link.html title=project.title url=project.url %}
{% endfor %}
