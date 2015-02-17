---
layout:  default
title:  Projects
---

Things I've been working on:
{% for post in site.posts %}
{% if post.categories contains 'projects' %}	
<div class="postHeader">
<a href="{{post.link}}">{{ post.title }}</a>
<img src="{{site.url}}/img/projects/{{post.image}}" />
</div>
{{post.excerpt}}
<a href="{{site.url}}/{{post.url}}">Link to post-mortem</a>
{% endif %}
{% endfor %}
