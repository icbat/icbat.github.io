---
layout:  default
title:  Art
---

{% for post in site.posts %}
{% if post.categories contains 'art' %}
<div class="postHeader">
<a href="{{site.url}}/{{post.url}}">{{ post.title }}</a>
</div>
<img src="{{site.url}}/{{post.categories}}/img/{{post.image}}.png" />
{{post.excerpt}}
{% endif %}
{% endfor %}
