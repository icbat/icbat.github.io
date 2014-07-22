---
layout:  default
title:  Minigames - Games made in a week or less!
---
{% for post in site.posts %}
{% if post.categories contains 'minigames' %}
<div class="postHeader">
	{{ post.categories }}:  <a href="{{ site.url }}/{{ post.url }}">{{ post.title }}</a> {{post.date | date: "%-d %B %Y"}}
</div>
{{post.excerpt}}
{{post.tags}}	
{% endif %}
{% endfor %}
