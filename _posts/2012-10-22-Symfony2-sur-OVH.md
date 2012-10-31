---
layout: post
title: Deployer une application Symfony 2 sur OVH
comments: true
category: article 
---

# Deployer une application Symfony 2 sur OVH

{% excerpt %}
Malgré des taux d'échecs importants, certains projets informatiques arrivent à terme. Dans certains cas, il faut les déployer en ligne. Et dans certains cas, il faut déployer chez OVH. Et dans certains de ces cas la, il faut déployer sur un serveur mutualisée sans accès SSH. Et ça, c'est pas intuitif.
{% endexcerpt %}

## Deployer le projet

Comme chez OVH, l'accès en SSH n'est pas garantie (présent uniquement sur les offres pro) et si votre client veut pas dépenser un sous, faut ruser pour installer le projet puisque Symfony a besoin d'un accès shell pour pouvoir télécharger les vendors. La solution la plus simple, c'est de télécharger toutes ses données en local puis d'envoyer le tout via FTP. Mettons à jour nos vendors :

<div class="syntax">
{% highlight bash %}
$ php bin/vendors install 
{% endhighlight %}
</div>


Et si vous avez utilisez Assetic dans votre projet, il faut aussi dumper ces ressources :

<div class="syntax">
{% highlight bash %}
$ php app/console assets:install web
$ php app/console assetic:dump --env=prod
{% endhighlight %}
</div>


Et la petite astuce, c'est que les fichiers git des vendors ne nous importent que peu pour un deploiement en production. On va donc supprimer tout les dossiers .git dans le dossier vendors ce qui va beaucoup alléger le projet : (de 149mo à 26mo)

<div class="syntax">
{% highlight bash %}
$ find . -type f | grep -i .git | xargs rm
{% endhighlight %}
</div>

Ensuite, on envoie le tout via FTP.


## Configuration OVH

Le serveur mis à notre disposition par OVH n'est pas compatible avec Symfony out-of-the-box. Il faut aller changer quelques parametres d'apache/php. Comme nous n'avons pas accès à la configuration Apache ou PHP de la machine, on est obligé de passer la voie du .htaccess. 

<div class="syntax">
{% highlight apacheconf %}
SetEnv SHORT_OPEN_TAGS 0
SetEnv REGISTER_GLOBALS 0
SetEnv MAGIC_QUOTES 0
SetEnv SESSION_AUTOSTART 0
SetEnv ZEND_OPTIMIZER 1
SetEnv PHP_VER 5_3
SetEnv SESSION_USE_TRANS_SID 0
{% endhighlight %}
</div>

Ensuite, il vous suffit d'aller modifier le fichier parameters.ini et c'est parti!

## Configuration projet

C'est presque terminé, il reste maintenant quelques derniers détails à corriger. 
 
* Comme il n'est pas possible de faire un <span class="syntax">clear:cache</span> sans ligne de commande, il faut le faire via le client FTP. Si vous voulez videz le cache, il suffti de supprimer/vider le dossier. Si le dossier est vide, Symfony va recalculer le cache à la prochaine requete. 

* Il faut aussi donner les droits au serveur pour qu'il puisse ré-écrire le cache. Il suffit de passer les dossiers app/logs et app/cache en 777. 

* Et toujours dans une optique de sécurité, pensez aussi à supprimer les fichiers de développement: **app_dev.php** et **config.php.** 
