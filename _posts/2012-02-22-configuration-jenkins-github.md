---
layout: post
title: Jenkins et Github
comments: true
category: article 
---

# Configuration de Jenkins et Github

Un billet intéressant pour ceux qui utilisent Jenkins ainsi que Github pour la construction de leur projets (je ferais un billet plus tard sur ce sujet). L'objectif ici est de configurer Jenkins afin qu'il lance un nouveau build quand des données ont été poussés sur Github. 

## 1. Configurer Jenkins

* La première chose à vérifier est la présence du **plugin Git**. Si ce n'est pas encore fait, il est disponible dans l'update center de Jenkins. Il suffit de l'installer et de redémarrer Jenkins pour que les modifications soient prises en compte. 

* Renseigner aussi les différentes options nécessaires pour Git. Branches à builder, récursivités des submodules et autres. 

* Dans la configuration de votre projet, il faut aller cocher : "Déclencher les builds à distance" et renseigner un token. C'est-à-dire une chaine aléatoire de caractères que vous pouvez génerer sur de nombreux sites sur le net. (Une petite préference pour celui-ci en ce qui me concerne). 

* Si votre Jenkins et privé, c'est-à-dire accéssible uniquement avec un couple login/mot de passe, il faut aller créer un utilisateur : **Github**. Si vous utilisez un système de droits matriciels, n'oubliez pas de donner les droits à l'utilisateur. Il lui faut au minimum Overall/read et Job/Read.


## 2. Configurer le serveur. 

Encore une fois, si votre repository sur Github est privé, vous aurez besoin d'installer les clé SSH pour que Jenkins puisse se connecter à Github. Rien de compliqué, juste ne pas oublier de générer la clé avec le user associé à Jenkins : 

<div class="syntax">
{% highlight bash %}
su jenkins
ssh keygen -t rsa -C monadresse@email.com
cat ~/.ssh/id_rsa.pub
{% endhighlight %}
</div>

Et n'oubliez pas d'ajouter ensuite cette clé dans votre compte Github. De plus, si c'est la première fois que votre serveur se connecte en SSH sur Github, sachez qu'il faut ajouter Github dans les known_hosts. Le plus simple, c'est d'executer ces commandes et d'accepter quand vous y êtes invités : 

<div class="syntax">
{% highlight bash %}
su jenkins
git -T git@github.com
{% endhighlight %}
</div>

## 3. Configurer Github. 

Dans Github, dans l'administration de votre projet, il y'a un onglet **Service Hooks**, ajoutez une **Post-Receive URL** :

<div class="syntax">
http://github:motdepasse@adresseJenkins:port/job/nomDuProjet/build?token=token
</div>

Petite astuce de cette URL, c'est qu'il est possible de passer les informations de connexion directement dans l'url. Ca va permettre d'utiliser un Jenkins protégé par un login/mot de passe sans pour autant bloquer tout action entrante. 

Et pour tester le tout, vous pouvez utiliser le bouton Test Hook dans Github qui va appeller cette URL et donc logiquement, si tout s'est bien passé, déclencher un build! 

