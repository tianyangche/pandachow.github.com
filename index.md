---
layout: page
title: Welcome!
tagline: You have to believe in youself when no one else does.
---
{% include JB/setup %}

## I am a
Christian | Hefeier | Entry level programmer

## I love
Mathematics | Parallel Computing | Country and Rock | Photography | Table Tennis

## Follow me @
[twitter](https://twitter.com/ailurus1991) | [douban](http://www.douban.com/people/ailurus1991/) | [github](https://github.com/pandachow)

Hey, guys! Please take this [gift](/assets/files/misc/gift.gif) as a pledge of our friendship. :-)

## Here are my five recent posts: 
{% for post in site.posts limit:5 %}
<li><span class="post_date">{{ post.date | date: "%B %e, %Y" }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}