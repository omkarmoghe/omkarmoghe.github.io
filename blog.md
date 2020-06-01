---
layout: default
title: Blog
---
<h1 class="blog"> {{ page.title }}</h1>

{% for post in site.posts %}
  {% if post.external_url %}
    {% assign url = post.external_url %}
  {% else %}
    {% assign url = post.url %}
  {% endif %}
  {% include menu_link.html title=post.title url=url %}
{% endfor %}
