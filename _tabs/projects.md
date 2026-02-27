---
icon: fas fa-project-diagram
order: 1
---

A collection of projects I'm currently working on.

---

{% assign projects = site.posts | where_exp: "post", "post.categories contains 'projects'" %}
{% for post in projects %}
### [{{ post.title }}]({{ post.url }})
{{ post.excerpt }}

{% endfor %}
