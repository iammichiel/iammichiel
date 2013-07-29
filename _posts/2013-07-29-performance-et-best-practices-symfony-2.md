---
layout: post
category: article 
title: Oracle et Symfony2, cauchemar en cuisine
comments: true
---

# Oracle et Symfony2, cauchemar en cuisine

{% excerpt %}

Deux technologies qui ont chacun une part de marché non négligeable. Oracle une société américaine qui veut se positionner sur tous les fronts, (cloud, middleware, integration, etc) qui possède son produit phare : Oracle Database qui est installé depuis la nuit des temps comme la base de données de réference des entreprises.

De l'autre coté, Symfony 2, un framework PHP plus jeune qui petit à petit devient une réference. Il suffit de regarder la vitesse d'adoption de Symfony 2 ainsi que les projets qui se basent dessus : Drupal, ez Publish, phpBB, etc. 

Deux technologies qui tendent à occuper une place de plus en plus importante dans le monde du développement et pourtant l'association des deux est un véritable cauchemar. 
{% endexcerpt %}


# Fais moi marcher!

La première difficulté n'est pas des moindres : faire une connexion depuis PHP vers Oracle. Dans la version actuelle des choses, il est impossible, peu importe la facon, d'installer le driver Oracle sur Mac. J'ai essayé une dizaine de solutions : homebrew, macports, source code, binaire traffiqués, rien n'a fonctionné. C'est finalement avec Vagrant que j'ai réussi.

Ensuite, avoir un poste de développement, c'est bien, avoir un environnement de production, c'est mieux. La cible : Red Hat Entreprise Linux. Choix très commun pour les grandes entreprises, mais pas commun pour les développeurs PHP. En effet, si le choix des entreprises se porte très souvent sur RHEL c'est à cause du support proposé par Red Hat. Un support plutôt poussé et efficace (d'après les retours que j'ai eu) à quelques conditions. Dont une qui pose problème : n'installer que des paquets (rpm) certifiés Red Hat. Sauf que, pas de chance, Oracle ne fournit aucun paquet pour le driver Php-Oracle, il faut le compiler à la main. Alors de facto, en compilant l'extension, le support de Red Hat saute. 

Bref, faites vous des copains chez les admins, annoncez un projet PHP-Oracle. Vous deviendrez ainsi leur nouveau satan. 


# Doctrine aime pas Oracle

Le sentiment est réciproque puisque Doctrine n'aime pas Oracle. C'est dans la logique des choses : Doctrine facilite la vie du développeur en réalisant un certain nombre d'opérations pour le développeur. Mais Oracle, c'est compliqué. Par exemple, une colonne auto-increment sur Oracle, ca n'existe pas. Il faut utiliser une séquence. Et on ne peut pas utiliser une séquence dans une insertion, ce qui finit sur des insertions de ce type : 

<div class="syntax">
{% highlight sql %}

SELECT ma_sequence.nextval FROM DUAL;
INSERT INTO ma_table (col1, [...] VALUES (?, ?, ?, ?, ?);

{% endhighlight %}
</div>

Oui, on fait bien deux requetes au lieu d'une seule pour une insertion. Pour des petits écrans tout va bien, pour des écrans avec 500 insertions, ca pèse. Mais, c'est pareil pour les languages me direz-vous, et bien oui, mais le driver PHP Oracle est mauvais, très mauvais en terme de performances. Du coup, ca devient un problème et il faut en tenir compte à la conception. 

Maintenant, faites vous des copains chez les Chef de projets, expliquez leur pourquoi la page s'exécute en 30 secondes et pas en moins d'une seconde comme le spécifie le cahier des charges. Vous deviendrez l'abruti qui a poussé Symfony 2 alors que le CP, lui, savait très bien que ca aurait du être fait en Struts 1.


# Performance ou bonnes pratiques?

Et dernier point que je voudrais aborder, (j'en passe et j'en passe) viendra un moment ou il vous faudra choisir : 

- Soit respecter vos convictions, vos croyances de développeurs inculqués à coup de "RTFM" ou "noob" sur les forums, martelés pendant les code-review par vos pairs ou encore preché par un "evangelist" pendant une conférence. 
- Soit accepter la réalité du monde du travail et coder "salement". 

J'ai sous mes yeux une pull-request où se pose cette question : la performance ou les best-practices? Un cas tordu, une situation particulière, me direz-vous? Mais non, une situation basique, un besoin basique et une performance x15 entre les deux implémentations.

### Voici la version propre (simplifié) : 

<div class="syntax">
{% highlight php startinline %}

$stmt = $con->prepare("[...] col1 = ?1 AND col2 = ?2");
$stmt->bindValue('1', $var1);
$stmt->bindValue('2', $var2);
$stmt->execute();

return $stmt->fetchAll(Query::HYDRATE_SINGLE_SCALAR);

{% endhighlight %}
</div>

Avec cette version propre, dans un premier temps, on prépare une requête SQL, puis on bind les variables pour la conversion du format (par exemple pour les dates), l'échappement contre les injections SQL et autres modifications. Le temps d'exécution de cette requete : **140 secondes**. 

### La version "sale" :

<div class="syntax">

{% highlight php startinline %}

$stmt = $con->prepare("[...] col1 = $VAR1 AND col2 = $VAR2 [...]");
$stmt->execute();

return $stmt->fetchAll(Query::HYDRATE_SINGLE_SCALAR);

{% endhighlight %}

</div> 

Dans cette version "sale", au lieu d'utiliser l'injection de variable, on se contente d'utiliser les variables en brut dans la requete. Grosse faille de sécurité, pas d'échappement, etc. Bref, du sale code. Temps d'execution : **9 secondes**. 

Voila. Les chiffres parlent d'eux mêmes. 


# Fuyez, pauvres fous

Si une seule conclusion devrait se présenter à cet article : Oracle, c'est bien. Symfony 2, c'est bien mais si vous devez combiner les deux, fuyez. 