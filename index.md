---
layout: default
title: DSA-LeetCode
---

# DSA-LeetCode

## Top Interviews

Welcome to the notes collection for core interview problems.

Use the sidebar to jump directly to a problem and read the solution on the right.

---

## Quick links

{% assign interval_pages = site.pages | where_exp: 'p', 'p.url contains "/Intervals/"' | sort: 'url' %}
{% for p in interval_pages %}
  {% unless p.url == '/' %}
- [{{ p.title | default: p.name }}]({{ p.url | relative_url }})
  {% endunless %}
{% endfor %}
