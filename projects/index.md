---
layout:  default
title:  Projects
---

Things I've been working on:
{% for post in site.posts %}
{% if post.categories contains 'projects' %}	
<div class="postHeader">
{{ post.date | date: "%Y-%m-%d" }} <a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
</div>
{{post.excerpt}}
{% endif %}
{% endfor %}
