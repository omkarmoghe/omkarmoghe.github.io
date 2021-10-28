---
layout: default
title: Blog
---
<h1 class="blog"> {{ page.title }}</h1>

Music, photography, miscellaneous projects, and random thoughts.

{% for post in site.posts %}
  {% if post.external_url %}
    {% assign url = post.external_url %}
  {% else %}
    {% assign url = post.url %}
  {% endif %}

  <div class="with-tag">
    {% if forloop.first == true %}
      {% include tag.html title="Latest" class="blog_alt" %}
      &nbsp;
    {% endif %}
    {% include menu_link.html title=post.title url=url %}
  </div>
{% endfor %}
