---
layout: default
title: Homepage
group: home
---

{% for post in site.posts limit: 3 %}

<article>
	<summary>
		<div class="day">{{ post.date | date: "%d") }}</div>
		<div class="month">{{ post.date | date: "%b") }}</div>
		<div class="year">{{ post.date | date: "%Y") }}</div>
	</summary>
    <section>
		<h1>{{ post.title }}</h1>
		{{ post.excerpt | markdownify }}
		<a class="readMore" href="{{ post.url }}">read more...</a>
	</section>
</article>

{% endfor %}
