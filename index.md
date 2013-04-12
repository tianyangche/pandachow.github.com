---
layout: page
title: Welcome!
tagline: You have to believe in youself when no one else does.
---
{% include JB/setup %}

## I am a
student | Christian | Entry level programmer | Hefeier

## I love
Mathematics | Country and Rock | Photography

## Follow me @
[twitter](https://twitter.com/ailurus1991) | [douban](http://www.douban.com/people/ailurus1991/) | [github](https://github.com/pandachow)

## Here are my recent blogs:
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>