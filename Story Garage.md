---
layout: page
title: "Story Garage"
permalink: /story/
main_nav: true
---

<p style="font-style: italic;">"This section covers all kinds of reviews, experiences, and more."</p>
<hr>

<ul class="posts-list">
  {% for post in site.categories["Story Garage"] %}
    <li>
      <strong>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </strong>
      <span class="post-date">- {{ post.date | date_to_long_string }}</span>
    </li>
  {% endfor %}
</ul>
<br>
