---
title: Projects
layout: inner
---

{% for project in site.projects %}
  <div class="project">
    <h2><a href="{{ project.url }}">{{ project.title }}</a></h2> <h6>{{project.lead_text}}</h6>
  </div>
{% endfor %}

&nbsp;





