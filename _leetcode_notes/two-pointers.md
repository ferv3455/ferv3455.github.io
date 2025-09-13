---
layout: post
title: "Two Pointers"
tag: two-pointers
---

# {{ page.title }}

## Strategy
Two pointers are useful for sorted arrays, strings, or linked lists...

## Selected Problems
The following problems can be effectively solved using the two pointers technique:
{% assign wanted_ids = "1,167" | split: "," %}
<ul>
{% for pid in wanted_ids %}
  {% assign prob = site.leetcode_problems | where: "leetcode_id", pid | first %}
  {% if prob %}
    <li><a href="{{ prob.url }}">{{ prob.title }}</a> (#{{ prob.leetcode_id }})</li>
  {% endif %}
{% endfor %}
</ul>

## All Problems
{% assign related = site.leetcode_problems | where_exp: "item", "item.tags contains 'two-pointers'" %}
<ul>
{% for prob in related %}
  <li><a href="{{ prob.url }}">{{ prob.title }}</a> (#{{ prob.leetcode_id }})</li>
{% endfor %}
</ul>
