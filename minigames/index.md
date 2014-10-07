---
layout:  default
title:  Minigames - Games made in a week or less!
---
{% for post in site.posts %}
{% if post.categories contains 'minigames' %}	
<div class="postHeader">
{{ post.title }}  <a href="{{ site.url }}{{ post.url }}">Read about the development</a>
</div>
{{post.excerpt}}
{% endif %}
{% endfor %}
