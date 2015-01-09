---
layout: page
title: Archive
---
Below is a listing of posts I have written in the past:

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}