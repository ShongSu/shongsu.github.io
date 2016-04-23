---
layout: page
title: Archives
description: "聽青春 迎來笑聲 羨煞許多人 那史冊 溫柔不肯 下筆都太狠"
header-img: "img/plane.jpg"
---

# {{ page.title }}

<ul>
    {% for post in site.posts %}

    {% unless post.next %}
    <h2>{{ post.date | date: '%Y' }}</h2>
    {% else %}
    {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
    {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
    {% if year != nyear %}
    <br>
    <h2>{{ post.date | date: '%Y' }}</h2>
    {% endif %}
    {% endunless %}

    <li><a href="{{ post.url }}">{{ post.title }}</a> &raquo; {{ post.date | date:"%B %e, %Y" }} </li>
    {% endfor %}
</ul>
