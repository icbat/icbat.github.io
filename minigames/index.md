---
layout:  default
title:  Minigames - Games made in a week or less!
---
{% for post in site.posts %}
{% if post.categories contains 'minigames' %}	
<div class="postHeader">
{{ post.date | date: "%Y-%m-%d" }} {{ post.title }}  <a href="{{ site.url }}{{ post.url }}">Read about the development</a>
</div>
{{post.excerpt}}
{% endif %}
{% endfor %}
