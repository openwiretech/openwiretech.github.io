---
#layout: page
layout: default
title: The Rants
permalink: /rants/
---


<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'rant' %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a> 
      </li>
    {% endif %}
  {% endfor %}
</ul>
