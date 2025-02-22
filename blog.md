---
title: Blog
layout: inner
---

{% assign sorted_posts = site.blog | sort: "publish_date" | reverse %}
{% for post in sorted_posts %}
  <div class="project">
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    <h6>{{ post.publish_date | date: "%d %B %Y" }}</h6>
  </div>
{% endfor %}


