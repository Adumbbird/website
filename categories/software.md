---
layout: category
title: software
permalink: /blog/categories/software/
picture: software.jpg
---

<h1> {{ page.title }} </h1>
<h5>Neat things </h5>
<p>I've come accross some pretty neat software that wasn't the easiest to set up. Here are the things I've figured out. I hope it helps you or atleast provides some form of entertainment.</p>
<div class="card">
{% for post in site.categories.software %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>