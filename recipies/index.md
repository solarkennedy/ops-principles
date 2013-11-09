---
title: All Recipies 
layout: default
---

{% for post in site.categories.recipies %}
 - [{{post.title}}]({{post.url}})
{% endfor %}
