---
title: Examples
permalink: /examples/
nav_order: 2
has_children: true
layout: page
---

## Examples

{% assign example_dirs = site.pages | where_exp: "item", "item.path contains 'examples/'" | where_exp: "item", "item.path != 'examples/index.md'" | where_exp: "item", "item.name == 'index.md'" | sort: "title" %}
{% if example_dirs.size > 0 %}
<div class="examples-grid">
  {% for example in example_dirs %}
  <div class="example-card">
    <h3><a href="{{ example.url | relative_url }}">{{ example.title }}</a></h3>
    <p>{{ example.description }}</p>
  </div>
  {% endfor %}
</div>
{% else %}
<div class="examples-grid">
  <div class="example-card">
    <h3>No Examples Found</h3>
  </div>
</div>
{% endif %}

<style>
.examples-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 30px;
}

.example-card {
  border: 1px solid var(--border-color);
  border-radius: 6px;
  padding: 20px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.example-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.example-card h3 {
  margin-top: 0;
  font-size: 1.4rem;
}

.example-card p {
  color: var(--text-color-light);
  margin-bottom: 20px;
}

.btn {
  display: inline-block;
  padding: 8px 16px;
  background-color: var(--link-color);
  color: white;
  text-decoration: none;
  border-radius: 4px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.btn:hover {
  background-color: var(--link-color-hover);
  text-decoration: none;
}
</style>
