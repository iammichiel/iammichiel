---
layout: default
title: Articles
group: "articles"
---

# Liste des articles

<ul>
	{% for post in site.categories.article %}
		<li>
			<a href="{{ post.url }}">{{ post.title }}</a>
		</li>
	{% endfor %}
</ul>