---
layout: page
title: "Code & Road"
permalink: /code/
main_nav: true
---

<h2 id="code-road">Code & Road</h2>
{% for desc in site.descriptions %}
  {% if desc.cat == "Code & Road" %}
    {{ desc.desc }}
  {% endif %}
{% endfor %}
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
