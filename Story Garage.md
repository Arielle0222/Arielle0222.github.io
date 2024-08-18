---
layout: page
title: "Story Garage"
permalink: /story/
main_nav: true
---

<h2 id="story-garage">Story Garage</h2>
{% for desc in site.descriptions %}
  {% if desc.cat == "Story Garage" %}
    {{ desc.desc }}
  {% endif %}
{% endfor %}
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
