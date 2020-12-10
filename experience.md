---
layout: default
title: Experience
---
<h1 class="experience">{{ page.title }}</h1>

Places I've worked, and orgs I've been a part of.

{% assign jobs = site.jobs | sort: "order" | reverse %}
{% for job in jobs %}
  <div class="with-tag">
    {% if forloop.first == true %}
      {% include tag.html title="Current" class="experience_alt" %}
      &nbsp;
    {% endif %}
    {% include menu_link.html title=job.title url=job.url %}
  </div>
{% endfor %}
