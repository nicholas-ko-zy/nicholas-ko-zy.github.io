---
title: Blog
layout: inner
---
# Blog

<ul>
  {% for post in site.posts %}
    <li style="display: flex; align-items: center;">
      <!-- Image on the left with adjusted positioning -->
      <div style="flex-shrink: 0; margin-right: 5px; margin-left: -50px;">
        <img src="/img/blog/{{post.img_filename}}" alt="Image for {{ post.title }}" style="max-width: 300px; height: auto;">
      </div>
      <!-- Post content on the right -->
      <div style="flex-grow: 1;">
        <a href="{{ post.url }}" style="font-size: 1.7em;">{{ post.title }}</a>
        <span style="margin-left: 10px; font-size: small;">
          {{ post.date | date: "%-d" }}{% case post.date | date: "%-d" %}{% when '1' or '21' or '31' %}st{% when '2' or '22' %}nd{% when '3' or '23' %}rd{% else %}th{% endcase %} {{ post.date | date: "%b %Y" }}
        </span>
        <p class="post-lead-text" style="margin-top: 5px; font-size: 1em;">
          {{ post.lead_text }}
        </p>
      </div>
    </li>
  {% endfor %}
</ul>




&nbsp;

<!-- {% for project in site.projects %}
  <div class="project">
    <h2><a href="{{ project.url }}">{{ project.title }}</a></h2> <h6>{{project.lead_text}}</h6>
  </div>
{% endfor %}

&nbsp; -->