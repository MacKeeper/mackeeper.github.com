---
layout: page
title: Maligue.ca
tagline: How we got there...
---
{% include JB/setup %}

Welcome to the dev blog of [Maligue.ca](https://www.maligue.ca).
    
## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>