---
layout:  default
title:  Minigames - Games made in a week or less!
---

<ul>
  {% for post in site.posts %}
  {% if post.category == 'Minigames' %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endif %}
  {% endfor %}
</ul>
