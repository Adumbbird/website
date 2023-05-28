---
layout: category
title: photography
permalink: /blog/categories/photography/
picture: nikon.png
---

<h1> {{ page.title }} </h1>
<h5> Capturing Memories</h5>
<p>I've been taking photos for a while now, but lately i've been stepping up the level i've been taking pictures at. I updated to the Nikon Z5 and have been taking my camera everywhere. I enjoy using Adobe Lightroom to slightly edit the photos. </p>
<div class="card">
{% for post in site.categories.photography %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>