---
layout: post
category: article
title: Generate a zip file in Symfony 2
comments: true
---

# Generate a zip file in Symfony 2

{% excerpt %}

For one of my last projects, I had to create a zip file containing all uploaded files for a given user. At first, I thought this would be quite difficult but after a little research, it is in fact quite easy when using the php implentation.

{% endexcerpt %}

# Native or PHP

My first reflex and it should be yours also, was searching on packagist for "zip" librairies. There are in fact two eligible candidate based on number of downloads/stars :

- alchemy/zippy with 11k downloads
- zetacomponents/archive with more than 38k downloads.

Having worked with ZetaComponents a couple of years ago when it was still called eZ Components, I was pretty sure this would be robust. The framework has been around for a couple of years and not all ideas are greate in Zeta 
