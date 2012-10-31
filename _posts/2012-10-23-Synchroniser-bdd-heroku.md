---
layout: post
title: Synchroniser ses BDD sur Heroku
comments: true
category: article 
---

# Synchroniser ses BDD sur Heroku

{% excerpt %}
Je travaille actuellement sur petite application de suivi de projet. Bien que l'application soit encore au stade de développement et qu'il manque encore une floppée de fonctionnalités avant que le projet ne soit viable, je m'en sers déjà. J'ai donc une version de "production" en ligne sur Heroku avec des données de "production" dans une base de données Postgresql. 

Maintenant quand je développe en local, j'aimerais bien avoir un jeu de données et tant qu'à faire, un jeu de données réel pour un peu plus de pertinence. L'idéal serait donc de pouvoir synchroniser ma base de données en local avec la base de données de production. Et uniquement dans le sens production -> développement. Voyons comment on peut faire ça. 
{% endexcerpt %}


## Backup!

La solution est relativement simple puisqu'il suffit de s'appuyer sur l'existant des deux côtés : 

* **Heroku** propose de son coté  un système de backup des bases postgresql
* **Postgresql** propose des commandes qui permettent d'importer en une ligne ces backups

Du côté d'Heroku, il faut installer l'addon avec la commande suivante : 

<div class="syntax">
{% highlight bash %}
$ cd /vers/le/projet/heroku
$ heroku addons:add pgbackups
{% endhighlight %}
</div>

Au moment ou j'écris ces lignes, la version gratuite de l'addon vous permets de faire 7 backups peu importe la durée de conservation. Dans le cas de la simple synchronisation, ca suffit amplement. Pour réaliser un backup, il suffit de le demander : 

<div class="syntax">
{% highlight bash %}
$ heroku pgbackups:capture
{% endhighlight %}
</div>

Et si tout s'est bien passé, le backup devrait apparaitre dans la liste : 

<div class="syntax">
{% highlight bash %}
$ heroku pgbackups
ID    Backup Time          Size    Database
----  -------------------  ------  -----------------------------------------
b001  2012/10/22 22:08.57  13.8KB  HEROKU_POSTGRESQL_ONYX_URL (DATABASE_URL)
{% endhighlight %}
</div>


## Download and restore

Il reste maintenant à récupérer ce dump et à remplacer la base de développement en local par ce dump. Toujours avec l'addon de Héroku, on va obtenir l'url pour le télécharger et enregistrer le dump dans un fichier que nous appelerons **db.dump** :

<div class="syntax">
{% highlight bash %}
$ curl -o -# `heroku pgbackups:url`
{% endhighlight %}
</div>

Et en tout dernier, il faut donc recréer la base de données en local. Comme, on ne veut une synchronisation que de la production vers le développement, le plus simple est de remplacer la base de données par le dump. Seulement avec postgresql, l'opération se fait en deux mouvements : 

1. Suppression de la base de données 
2. Création d'une nouvelle base de données

Et seulement ensuite, on pourra injecter les données avec **pg_restore** :

<div class="syntax">
{% highlight bash %}
$ sudo -u postgres dropdb mydatabase
$ sudo -u postgres createdb mydatabase with owner myuser
$ sudo -u postgres pg_restore -O -d mydatabase db.dump
{% endhighlight %}
</div>

Et voilà. Ready to go.