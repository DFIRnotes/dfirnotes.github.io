---
layout: page
title: Posts
permalink: /posts/
---

posts loop:
{% for post in site.posts %}
<li>
	<a href="{{ post.url }}">{{ post.title }}</a> from {{ post.date }}
</li>
{% endfor %}
