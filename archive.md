---
title: Archive
hidden: true
---

{% for page in site.pages %}
  {% if page.title and page.hidden == false %}

<div class='post-title'>
  <h4><a href="{{ page.url }}">{{ page.title }}</a></h4>
  <p>{{ page.description }}</p>
</div>

  {% endif %}
{% endfor %}