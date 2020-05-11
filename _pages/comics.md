---
layout: archive
permalink: /comic/
author_profile: true
title: "Ivy's Comics"
header:
     image: "/assets/images/comic_header.jpg"
---

Navigate through the different comics I've created here:

## List of Comics
<div class="grid__wrapper">
    {% for post in site.posts %}
        {% if post.categories contains 'comics' %}
            {% include archive-single.html type="grid" %}
        {% endif %}
    {% endfor %}
</div>
<!-- <div class="grid__wrapper">
    {% for post in site.posts %}
        {% if post.categories contains 'comics' %}
            {% include archive-single.html type="grid" %}
        {% endif %}
    {% endfor %}
</div> -->
<!-- <div class="grid__wrapper">
  {% assign collection = 'comics' %}
  {% assign posts = site[collection] | reverse %}
  {% for post in posts %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div> -->
