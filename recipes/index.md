---
title: All Recipes 
layout: default
---

# All Recpies

{% for post in site.categories.recipes %}
 - [{{post.title}}]({{post.url}})
{% endfor %}
