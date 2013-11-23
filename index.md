---
layout: page
title: Hello World!
tagline:
---
{% include JB/setup %}

Hi all, 

Can't have a index page without a bit of intro, so here it is. Not much here 
except for a few posts on problem solutions I think other people might find useful. 

-----

{% for post in site.posts %}
  <div class="pull-right">{{ post.date | date_to_string }}</div>
  <h4><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h4>

  <p class="description">{{ post.description }}</p>
{% endfor %}
