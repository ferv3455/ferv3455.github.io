---
layout: home
title: "Personal Notes"
---

<style>
  details {
    margin-bottom: 1em;
    padding-left: 24px;
  }

  summary {
    cursor: pointer;
    padding: 8px 24px;
    margin-left: -24px;
    font-size: 20px;
    border: 1px solid #ccc;
    border-radius: 4px;
    transition: background-color 0.3s;
  }

  summary:hover {
    background-color: #f0f0f0;
  }

  ul {
    margin-top: 8px;
  }
</style>

## Computer Science Fundamentals

<details>
  <summary>Intro to Computer Systems</summary>
  <ul>
    {% for doc in site.cs_fundamentals %}
      <li>
        <a href="{{ doc.url | relative_url }}"
            {% if page.url == doc.url %}class="active"{% endif %}>
          {{ doc.title }}
        </a>
      </li>
    {% endfor %}
  </ul>
</details>


## Programming Languages

{% assign notebooks = "cpp_primer:C++ Primer,fluent_python:Fluent Python,effective_python:Effective Python,learning_go:Learning Go" | split: "," %}

{% for pair in notebooks %}
  {% assign parts = pair | split: ":" %}
  {% assign key = parts[0] %}
  {% assign label = parts[1] %}
  
  <details>
    <summary>{{ label }}</summary>
    <ul>
      {% assign docs = site[key] | sort: 'chapter' %}
      {% for doc in docs %}
        <li>
          <a href="{{ doc.url | relative_url }}"
              {% if page.url == doc.url %}class="active"{% endif %}>
            {{ doc.title }}
          </a>
        </li>
      {% endfor %}
    </ul>
  </details>
{% endfor %}


## Data Structures & Algorithms

{% assign notebooks = "cracking_the_coding_interview:Cracking the Coding Interview,random_notes_of_coding:Random Notes of Coding" | split: "," %}

{% for pair in notebooks %}
  {% assign parts = pair | split: ":" %}
  {% assign key = parts[0] %}
  {% assign label = parts[1] %}
  
  <details>
    <summary>{{ label }}</summary>
    <ul>
      {% assign docs = site[key] | sort: 'chapter' %}
      {% for doc in docs %}
        <li>
          <a href="{{ doc.url | relative_url }}"
              {% if page.url == doc.url %}class="active"{% endif %}>
            {{ doc.title }}
          </a>
        </li>
      {% endfor %}
    </ul>
  </details>
{% endfor %}


## Miscellaneous

<details>
  <summary>Misc Notes</summary>
  <ul>
    {% for doc in site.miscellaneous %}
      <li>
        <a href="{{ doc.url | relative_url }}"
            {% if page.url == doc.url %}class="active"{% endif %}>
          {{ doc.title }}
        </a>
      </li>
    {% endfor %}
  </ul>
</details>

