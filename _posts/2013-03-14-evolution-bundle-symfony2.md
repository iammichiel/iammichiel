---
layout: post
category: article 
title: EvolutionBundle, Continuous Deployment
comments: true
---

# EvolutionBundle, Continuous Deployment

{% excerpt %}

Depuis quelques temps, je m'intéresse à un toutes ces bonnes pratiques qui nous sont vantées un peu partout : l'agile, le continous deployment, le BDD (Behaviour Driven Development), etc. Pourtant, la mise en place de ces techniques n'est pas toujours évidente dans un cadre "entreprise". Il faut y aller par pétites étapes et [l'EvolutionBundle][evolutionbundle-github] est un outil crée pour l'une de ces bonnes pratiques : **le Continuous Deployment**.

{% endexcerpt %}


## Continuous Deployment

Le Continuous Deployement en soit n'a pas de sens sans ses deux prédécesseurs : Continuous Integration et Continuous Delivery (que je n'expliquerais pas ici). Si on traduit litérallement l'expression, ca veut dire : *déploiement continue* ce qui veut un peu tout et rien dire. Mon intérprétation personnelle du **Continuous Deployement**, c'est de pouvoir deployer le plus rapidement possible et le plus souvent possible. 

Et être capable de tendre vers cet idéal, ca implique : 

- Déployer avec le moins d'intéraction humaine possible
- Déployer avec le moins d'intérruption de service pour l'utilisateur

## Automatiser

Pour atteindre cela, il faut donc automatiser le plus possible les déploiements. Pour les applications Symfony, je suis un grand adepte de [Capistrano][capistrano-github]/[Capifony][capifony-site] qui s'occupe de la plus grande partie du déploiement pour nous. 

Par contre, une des choses à laquelle on est forcément confronté à un moment donné, c'est l'évolution de la base de données. On peut par exemple ajouter un deuxième champ *téléphone* dans la table *utilisateurs*. Cela implique que le déploiement se fait donc en au moins 2 parties : 

1. Je déploie la nouvelle version du code 
2. Je mets à jour la base de données 

Que faire dans ce cas si le deploiement pour une raison X ou Y a planté? Il manque une librairie? Un problème de droits sur le serveur? C'est la qu'intervient EvolutionBundle.

## EvolutionBundle

C'est tout simplement en étant confronté à ce genre de problème que l'on a cherché comment résoudre le problème. Il fallait être capable de :

- Modifier la structure d'une BDD
- Modifier les données de la BDD
- Avoir un mécanisme de rollback 

Il existait la solution de [Doctrine Migrations][doctrine-migrations] mais elle ne nous a pas convaincu. Trop complexe, une génération automatique et une documentation inexistante ainsi qu'une base d'utilisateurs très limitée.

C'est [Playframework][playframework-evolutions] qui nous inspira pour la solution. En effet, Play utilise une solution appelé Evolutions qu'on a décidé de la porter vers Symfony. 

## Evolution

Le principe du EvolutionBundle est vraiment simple et peut être résumé en une seule phrase : **le bundle execute des scripts SQL dans un ordre donnée.**

### Ordre
Chaque fichier est numéroté de la facon suivante : 1.sql, 2.sql, etc. Le bundle va executer les fichiers dans l'ordre. 

### Historique
Comme chaque evolution ne doit être appliqué qu'une seule fois, le bundle enregistre quels fichiers ont déjà été éxecutés. Il ne va executer que les nouveaux scripts SQL. 

Par exemple :
- Lors du déploiement 1.0, il execute les scripts présent, (1.sql, 2.sql). 
- Ensuite lors du déploiement 1.1, il n'éxecute que les nouveaux scripts : 3.sql, 4.sql.

### Rollback
Chaque fichier est coupé en deux parties. Une partie UP et une partie DOWN. Si au cours du déploiement un des scripts SQL est en erreur, le bundle va rollback les script executés en executant le code SQL contenu dans la partie down du fichier. 

Par exemple : 

<div class="syntax">
{% highlight sql %}
UP : ALTER TABLE logs ADD COLUMN exported BOOLEAN DEFAULT FALSE;
DOWN : ALTER TABLE logs DROP COLUMN exported;
{% endhighlight %}
</div>


Pour terminer, je dirais que le bundle est stable et utilisable mais il continuera d'évoluer pour s'adapter au problèmes rencontrées. Pour information, il est utilisé en production sur 4 applications chez un acteur de la grande distribution. Le bundle est sur [Github][evolutionbundle-github] et [packagist][evolutionbundle-packagist]!


[evolutionbundle-github]: https://github.com/lilweb/EvolutionBundle
[capistrano-github]: https://github.com/capistrano/capistrano
[capifony-site]: http://capifony.org/ 
[doctrine-migrations]: http://www.doctrine-project.org/projects/migrations.html
[playframework-evolutions]: http://www.playframework.com/documentation/2.1.1/Evolutions
[evolutionbundle-packagist]: http://packagist.org/packages/lilweb/evolution-bundle