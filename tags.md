---
layout: page
title: Tag
permalink: /tag/
---
{% comment %}
==============
This is to make the tags sorted by their name.
site.tags[0]: tag name
site.tags[1]: array of posts in this tag
==============
{% endcomment %}
{% assign ordered_tags = site.tags | sort %}
{% for tag in ordered_tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% assign posts_sorted = tag[1] | sort: 'chapter_number' | reverse %}
    {% for post in posts_sorted %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}

