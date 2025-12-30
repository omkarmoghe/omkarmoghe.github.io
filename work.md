---
layout: default
title: Work
---

<h1 class="work glow-h">{{ page.title }}</h1>

Places I've worked, and orgs I've been a part of.

{% assign jobs = site.jobs | sort: "order" | reverse %}
{% for job in jobs %}

  <div class="with-tag">
    {% if forloop.first == true %}
      {% include tag.html title="Current" class="work_alt" %}
      &nbsp;
    {% endif %}
    {% include menu_link_with_tag.html tag=job.emoji title=job.title url=job.url %}
  </div>
{% endfor %}
