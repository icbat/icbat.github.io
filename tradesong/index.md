---
layout:  default
title:  Tradesong Devblogs
---
{% for post in site.posts %}
{% if post.categories contains 'tradesong' %}	
<div class="postHeader">
<a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
</div>
{{post.excerpt}}
{% endif %}
{% endfor %}
