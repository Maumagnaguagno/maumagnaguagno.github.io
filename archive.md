---
title: Archive
hidden: true
---

{% for page in site.pages %}
  {% unless page.hidden %}

<div>
  <span class='post-title'>
    <h4><a href="{{ page.url }}">{{ page.title }}</a></h4>
    <p>{{ page.description }}</p>
  </span>
</div>

  {% endunless %}
{% endfor %}