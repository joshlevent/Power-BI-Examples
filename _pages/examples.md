---
title: Examples
permalink: /examples/
nav: true
nav_order: 2
---

# Power BI Examples

{% assign example_dirs = site.pages | where_exp: "item", "item.path contains 'examples/'" | where_exp: "item", "item.name == 'index.md'" | sort: "title" %}
{% if example_dirs.size > 0 %}
  {% for example in example_dirs %}
## [{{ example.title }}]({{ example.url | relative_url }})
{{ example.description }}
  {% endfor %}
{% else %}
## [Add Sunday Values to Monday](/Add%20Sunday%20Values%20to%20Monday/)
Add Sunday's values to Monday's data.
{% endif %}
