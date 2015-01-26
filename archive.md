---
layout: page
title: Archive
---
Here is a listing of all posts I have written in the past:

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}

And here are my code snippets:

{% for post in site.tags.code_snippet %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}