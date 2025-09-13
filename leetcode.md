---
layout: home
title: "LeetCode Problems"
---

Work in progress.

<!-- <table>
    <thead>
        <tr>
            <th>ID</th>
            <th>Problem</th>
            <th>Difficulty</th>
            <th>Tags</th>
        </tr>
    </thead>
    <tbody>
        {% assign sorted_problems = site.leetcode_problems | sort: "leetcode_id" %}
        {% for problem in sorted_problems %}
        <tr>
            <td>{{ problem.leetcode_id }}</td>
            <td><a href="{{ problem.url }}">{{ problem.title }}</a></td>
            <td>{{ problem.difficulty }}</td>
            <td>
                {% for tag in problem.tags %}
                    {% assign note = site.leetcode_notes | where: "tag", tag | first %}
                    {% if note %}
                        <a href="{{ note.url }}">{{ tag }}</a>{% if forloop.last == false %}, {% endif %}
                    {% else %}
                        {{ tag }}{% if forloop.last == false %}, {% endif %}
                    {% endif %}
                {% endfor %}
            </td>
        </tr>
        {% endfor %}
    </tbody>
</table> -->
