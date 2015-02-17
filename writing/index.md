---
layout:  default
title:  Writing
---
{% for post in site.posts %}
{% if post.categories contains 'dev' %}	
<div class="postHeader">
{{ post.date | date: "%Y-%m-%d" }} <a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
</div>
{{post.excerpt}}
{% endif %}
{% endfor %}
