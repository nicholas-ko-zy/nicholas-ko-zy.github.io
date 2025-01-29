---
title: Blog
layout: inner
---

{% for post in site.blog %}
  <div class="project">
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2> <h6>{{post.publish_date}}</h6>
  </div>
{% endfor %}

&nbsp;

