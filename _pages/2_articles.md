---
#layout: page
layout: default
title: The Articles
permalink: /article/
---

<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'article' %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a> 
      </li>
    {% endif %}
  {% endfor %}
</ul>
