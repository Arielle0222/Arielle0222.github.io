---
layout: page
title: "Code & Road"
permalink: /code/
main_nav: true
nav_order : 3
---

<p style="font-style: italic;">"This section focuses on programming, tutorials, and tech insights."</p>
<hr>

<ul class="posts-list">
  {% for post in site.categories["Code & Road"] %}
    <li>
      <strong>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </strong>
      <span class="post-date">- {{ post.date | date_to_long_string }}</span>
    </li>
  {% endfor %}
</ul>
<br>
