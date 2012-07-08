---
layout: post
category: article 
title: Environnement de développement Symfony 2 sur Mac (bis)
---

# Environnement de développement Symfony 2 sur Mac

Pour continuer la série du post que j'ai fait précedemment, il reste encore quelques petites manipulations à faire avant de pouvoir commencer un projet sur Symfony2. Voici les différentes étapes qu'il faudra répéter pour chaque projet. 

## Télécharger Symfony 2 sans vendors

Direction le site de [Symfony2](http://symfony.com/download) pour aller télécharger la dernière version **sans vendors**. Pourquoi sans vendors? Parce que ainsi, nous pourrons mettre à jour beaucoup plus facilement les vendors directement avec les commandes fournies par Symfony. 

Il faut comprendre que Symfony2 est un ensemble de libraires (vendors) rassemblées en un framework. Mais chaque librairie avance à son propre rythme. Par exemple, [Twig](http://twig.sensiolabs.org), le moteur de template de Symfony2. Si demain, une nouvelle version de Twig sort, il faudrait aller la télécharger et la décompresser dans le bon dossier à la main. Avec le script de mise à jour fourni par Symfony2, il suffit d'éxécuter une seule commande pour mettre à jour tout les "vendors" d'un coup. C'est beaucoup plus simple et rapide.

## Choix du répertoire

Ensuite, il faut décompresser l'archive dans le dossier web. Personnellement, je travaille dans **/files/web** qui est le root de mon serveur web. La seule consigne pour choisir un dossier, c'est que le chemin pour y accéder soit sans espaces et simple. Vous allez devoir y accéder régulièrement en ligne de commande, autant qu'il soit simple et court donc. 

## Installation du vhost

Il est aussi possible de mettre en place un nom de domaine plus explicite pour le projet. Pour cela, il faut aller créer un virtualhost pour apache dans le fichier : 
**/etc/apache2/extra/httpd-vhosts.conf**

Ajouter dans ce fichier les lignes suivantes : 

<div class="syntax">
{% highlight apache %}
<VirtualHost *:80>
	DocumentRoot "/files/web/monprojet/web"
	ServerName monprojet.site
  	ServerAlias monprojet.site
</VirtualHost>
{% endhighlight %}
</div>

Cette instruction va dire à Apache que un site a été installé dans le repértoire **/files/web/monprojet/web** et que toute requête pour le nom de domaine **monprojet.site** doit être redirigé vers ce dossier.

Il faudra aussi redémarrer le serveur Apache : 

<div class="syntax">
{% highlight bash %}
sudo apachectl restart
{% endhighlight %}
</div>

__Ajout dans le fichier hosts__

Ensuite, il faut que l'ordinateur sache aussi que les requetes sur **monprojet.site** doivent être redirigées vers le serveur apache local et non pas sur un serveur distant sur internet. Pour ça, il rajouter dans le fichier **/etc/hosts** cette ligne :

<div class="syntax">
{% highlight bash %}
127.0.0.1	monprojet.site
{% endhighlight %}
</div>

Tout requête sur le nom de domaine monprojet.site sera donc redirigé vers 127.0.0.1, c'est-à-dire la machine locale. 

Simple précision, il vaut mieux éviter l'extension **.local**. Ca parait pourtant logique d'utiliser cette extension la pour des sites en local, mais sur Mac OS, le service *bonjour* intercepte les requetes avec une extension .local. Ce qui peut provoquer des temps de chargement très long pour accéder au site. D'ou l'extension .site, qui elle, n'est pas interceptée. 


## Installation de la base de données

Il ne reste plus qu'à définir les identifiants de connexion dans Symfony2. Ils sont définis dans le fichier **app/config/parameters.ini**. De préférence, il faudra créer un utilisateur par site avec des droits uniquement sur sa propre base de données. Ca peut éviter qu'il ne modifie ou corrompe d'autre bases par erreur.


## Mettre à jour le .gitignore

Si vous utilisez Git pour versionner le code source, il faut  mettre à jour le .gitignore afin de ne pas versionner les fichiers qui ne nous interessent pas. 

Voici un exemple de gitignore pour Symfony2 :

<div class="syntax">
{% highlight bash %}
/web/bundles/
/app/bootstrap*
/app/cache/*
/app/logs/*
/vendor/
/app/config/parameters.ini
{% endhighlight %}
</div>


## Installation des vendors

Dernière étape avant de pouvoir commencer à coder, l'installation des vendors. Les dépendences de Symfony2 sont définies dans le fichier **deps**. Il donne le nom de la librairie ainsi que l'adresse de son repository Git. Pour installer ces libraires : 

<div class="syntax">
{% highlight bash %}
$ php bin/vendors install
{% endhighlight %}
</div>

Et vous pouvez commencer à développer !








