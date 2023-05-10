---
layout: page
title: website
permalink: /blog/categories/website/
---

<h1> {{ page.title }} </h1>
<h5> This is a project.</h5>
<p>I wanted to create a webpage for people to interact with me in a few different ways. I also wanted a good place to document my experiences with navigating the joys of running a company.</p>
<div class="card">
{% for post in site.categories.website %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>