---
layout: page
title: Posts
permalink: /posts/
---


posts:
{% for post in site.posts %}
<li>
	<a href="{{ post.url }}">{{ post.title }}</a> by {{ post.author }} from {{ post.date }}
</li>
{% endfor %}
