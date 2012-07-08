---
layout: post
title: Environnement de développement PHP sur Mac OS
category: article
---

# Environnement de développement PHP sur Mac OS

Pour travailller sur des projets PHP avec Symfony sur Mac, il faut mettre en place un environnement de développement. C'est à dire installer quelques applications, logiciels dont on va se servir. Le stricte minimum va être ceci : 

* Un serveur web
* Un interpreteur PHP
* Une base de données


## Serveur web

Pour le serveur web, pas trop de questions à se poser, [Apache2](http://httpd.apache.org/) sera le choix par défaut de beaucoup de personnes et ils ont raison. Un des principaux objectifs en mettant en place un environnement de développement est justement d'avoir un environnement qui se rapproche au maximum du serveur de production final. Cela nous évite les surprises de dernières minutes lors de la mise en production dû à des différences de versions, d'extensions ou autres. Or dans une très grande majorité des cas, le serveur de production final sera un serveur Apache2 donc autant utiliser celui-ci en développement. ([65% des parts de marché en 2011](http://news.netcraft.com/archives/2011/11/07/november-2011-web-server-survey.html))

La bonne nouvelle est que sur Mac Os, il y'a un serveur Apache2 installé avec le système d'exploitation. Pour le vérifier, dans le terminal, on peut faire ceci : 

<div class="syntax">
{% highlight bash %}
$ httpd -v
Server version: Apache/2.2.20 (Unix)
Server built:   Sep  8 2011 18:13:15
{% endhighlight %}
</div>

Ensuite, il est possible de lancer le procéssus avec la commande suivante et accéder ensuite à la page 
[http://localhost/](http://localhost) pour vérifier que le serveur fonctionne. 

<div class="syntax">
{% highlight bash %}
$ sudo apachectl start
{% endhighlight %}
</div>



## PHP

En ce qui concerne PHP, la version fournie avec Mac Os est déconseillée. D'une part, il s'agit de la version 5.2.x. Or Symfony2 par exemple, demande au moins une versions 5.3.2 minimum (pour l'utilisation des namespaces notamment). En plus, certaines extensions nécessaires pour le bon fonctionnement de Symfony2 ne sont pas incluses. Notamment la fameuse extension *intl* qui sera nécessaire pour l'internationalisation. 

Bref, [Liip](http://www.liip.ch), une boite suisse travailllant dans le domaine web a fait un [petit script sympa](http://php-osx.liip.ch/) qui se propose de faire toute cette installation/configuration pour vous. (Merci à eux!) Si vous n'avez pas envie de lire l'article, sachez qu'il suffit d'éxécuter la commande suivante pour tout faire : 

<div class="syntax">
{% highlight bash %}
$ curl -s http://php-osx.liip.ch/install.sh | bash -
{% endhighlight %}
</div>


## Base de données

Au niveau base de données, un choix doit être fait mais celui-ci encore une fois est plutôt facile. MySql sera le gagnant pour la plupart des besoins. Faisons tout de même un rapide tour d'horizon : 

* **Oracle** : Très performant, très spécialisé possède même son propre langage de programmation : le PL/SQL et surtout très très très cher. (et surtout pas vraiment compatible avec du PHP)
* **Postgresql** : Plutôt performant aussi, un peu plus poussé que MySQL, moins outillé, moins facile d'accès, demande un peu plus de connaissances à maintenir et à manipuler
* **MySQL** : Le plus répandu, le plus outillé, efficace sur la plupart des besoins
* **NoSQL** : Il reste toute la grappe des bases NoSQL telles que MongoDB, Reddis, Voldemort, Cassandra, mais ils ne conviennent que dans certains cas particuliers. 

Pour installer MySql, il faut se rendre sur le [site officiel](http://www.mysql.fr) dans la section download et prendre un DMG (je préfère toujours la version avec installeur). Tout au long du processus, l'installeur vous guidera et au bout de quelques minutes, il sera installé! 



Voila qui finalise donc l'environnement de développement. Un serveur http pour traiter les requêtes entrantes, PHP pour interpréter les instructions réçues et MySql pour stocker les informations de manières persistantes. Prochain billet, mise en place d'un projet Symfony2 dans ce même environnement de développement. 


