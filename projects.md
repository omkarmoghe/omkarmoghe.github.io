---
layout: default
title: Projects
---
<h1 class="projects"> {{ page.title }}</h1>

Cool stuff I'm working on.

{% for project in site.projects %}
  {% include menu_link.html title=project.title url=project.url %}
{% endfor %}
