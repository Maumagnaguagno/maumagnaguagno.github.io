---
title: Archive
hidden: true
---

{% for page in site.html_pages %}
  {% unless page.hidden %}

<div class='post-title'>
  {% if page.date %}
    <p style="font-size:small;float:right;">{{ page.date | date_to_long_string }}</p>
  {% endif %}
  <h4><a href="{{ page.url }}">{{ page.title }}</a></h4>
  <p>{{ page.description }}</p>
</div>

  {% endunless %}
{% endfor %}