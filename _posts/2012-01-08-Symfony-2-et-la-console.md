---
layout: post
title: Symfony2 et la console, quelques astuces
comments: true
category: article
---

# Symfony2 et la console, quelques astuces

En travaillant avec Symfony 2 (ou avec d'autres framework PHP) certaines choses doivent se faire avec la console. Comme la création de la base de données ou encore le rechargement des fixtures. Au final, la console c'est toujours l'ami du développeur. Que ce soit pour aller modifier un paramètre dans le php.ini ou encore pour aller créer un host pour apache, au final, la console sert à beaucoup de choses et on ne peut pas passer à côté. 

Dans le cas de Symfony 2, certaines commandes vont être executées assez régulièrement. Par exemple, en créant une nouvelle entité, on crée aussi les fixtures associées. Après, il faut recharger la base de données, mettre à jour le modèle puis recharger les fixtures. Ce qui revient à executer ceci : 

<div class="syntax">
{% highlight bash %}
$ php app/console doctrine:schema:update --force
$ php app/console doctrine:fixtures:load 
{% endhighlight %}
</div>

Premier constat, chacune de ces deux commandes commence par la même chose <code>php app/console</code>. Dans Symfony 2, les commandes commencent soit par <code>app/console</code> ou bien par <code>bin/vendors</code>. La deuxième commande étant uniquement utilisée dans le cas de l'ajout d'un nouveau bundle ou la mise à jour des bundles. Commençons par créer un alias pour cette première commande : 

<div class="syntax">
{% highlight bash %}
$ alias sf="php app/console"
{% endhighlight %}
</div>

Le rechargement de la base de données au complet peut-être simplifié. Résumons toutes les commandes dans une seule avec un alias. Ce qui nous donne ceci :

<div class="syntax">
{% highlight bash %}
 $ alias sf-db="sf doctrine:schema:update --force; sf doctrine:fixtures:load;"
{% endhighlight %}
</div>

Mais comme il arrive que Doctrine ne supprime pas les anciennes tables et que j'aime travailler sur une base de données clean, ajoutons deux étapes que sont la suppression et la création de la base de données : 

<div class="syntax">
{% highlight bash %}
$ alias sf-db="sf doctrine:database:drop --force; sf doctrine:database:create; sf doctrine:schema:update --force; sf doctrine:fixtures:load;"
{% endhighlight %}
</div>

Il est aussi fréquent de devoir faire clear cache qui supprime les différentes choses mises en cache et recrée le cache. Nécéssaire quand on renomme une vue ou un controlleur par exemple. Voici un alias qui permets de simplifier tout ca : 

<div class="syntax">
{% highlight bash %}
$ alias sf-cl="cache:clear"
{% endhighlight %}
</div>

Et ajoutons une dernière commande qui fait tout ca : 

<div class="syntax">
{% highlight bash %}
$ alias sf-all="sf-db; sf-cl;"
{% endhighlight %}
</div>

ce qui nous fait donc 4 aliases pour nous simplifier la vie avec Symfony 2  : 

* **sf** : qui permets d'éxécuter tout les commandes 
* **sf-db** : qui recrée la base de données et recharge les fixtures
* **sf-cl** : qui supprime le cache
* **sf-all** : qui recrée la base de données et qui va recréer le cache


PS : Il suffit de copier ça dans votre fichier bash. Pour les Mac, ca se trouve dans <code>~/.bash_profile</code> et pour les Linux, ça se passe dans <code>~/.bashrc</code>. Pour les Windowsiens... osef! 



