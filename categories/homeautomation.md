---
layout: page
title: automation
permalink: /blog/categories/automation/
---

<h1> {{ page.title }} </h1>
<h5> Custom Smart Devices</h5>
<p>Ever since watching Smart House as a kid, i've always wanted to have a smart home. Here are tales of my adventures in the quest to have a connected home.</p>
<div class="card">
{% for post in site.categories.automation %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>