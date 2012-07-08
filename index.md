---
layout: default
title: Homepage
group: home
---

{% assign first_post = site.posts.first %}

<summary>
	<div class="day">{{ first_post.date | date: "%d") }}</div>
	<div class="month">{{ first_post.date | date: "%b") }}</div>
	<div class="year">{{ first_post.date | date: "%Y") }}</div>
</summary>

<article>
	{{first_post.content}}
</article>