---
layout:  default
title:  Games
---

{% for post in site.posts %}
{% if post.categories contains 'games' %}
<div class="postHeader">
<a href="{{site.url}}/{{post.url}}">{{ post.title }}</a>
</div>
<img class="thumbnail" src="{{site.url}}/{{post.categories}}/img/{{post.image}}.png" />
{{post.excerpt}}
{% endif %}
{% endfor %}
