---
layout:  default
title:  Writing
---
{% for post in site.posts %}
{% if post.categories contains 'writing' %}	
<div class="postHeader">
<a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
</div>
{{post.excerpt}}
{% endif %}
{% endfor %}
