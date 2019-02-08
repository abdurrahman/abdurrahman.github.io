---
layout: page
title: Archive
---

<div class="posts">
  {% for post in site.posts %}
    <span class="post-title">
      <a href="{{ site.baseurl }}{{ post.url }}">
        {{ post.title }}
      </a>
    </span>
    <span class="post-date">{{ post.date | date_to_string }}</span>
  {% endfor %}
</div>