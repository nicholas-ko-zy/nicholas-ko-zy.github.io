---
title: Blog
layout: inner
---

{% assign sorted_posts = site.posts | sort: "date" | reverse %}
{% for post in sorted_posts %}
  <div class="project">
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    <h6>{{ post.date | date: "%d %B %Y" }}</h6>
  </div>
{% endfor %}


