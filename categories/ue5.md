---
layout: category
title: ue5
permalink: /blog/categories/ue5/
picture: ue5.png
---

<h1> {{ page.title }} </h1>
<h5> This is game development.</h5>
<p>I'd like a place to post different things I've figured out about game development. Whether it's something to do with a game itself, or how I went about find a solution to a problem I faced. I like to use Unreal Engine 5 and since I'm not great at art, I'll mostly be using free assets from Epic or other sources. I'll provide links to resources I use within the post.</p>
<div class="card">
{% for post in site.categories.ue5 %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>