---
title: Projects
layout: inner
---

{% for project in site.projects %}
  <div class="project">
    <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>
  </div>
{% endfor %}

&nbsp;





