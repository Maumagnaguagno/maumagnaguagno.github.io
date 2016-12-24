---
title: Archive
hidden: true
---

{% assign sorted_pages = site.html_pages | sort: 'date' %}
{% for page in sorted_pages %}
  {% unless page.hidden %}

<div>
  {% include page_info.html %}
  <h4><a href="{{ page.url }}">{{ page.title }}</a></h4>
  <p>{{ page.description }}</p>
</div>

  {% endunless %}
{% endfor %}