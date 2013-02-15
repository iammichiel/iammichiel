---
layout: post
category: article 
title: De l'importance de l'ergonomie
comments: true
---

# De l'importance de l'ergonomie

{% excerpt %}

Un point trop souvent négligé dans le développement est l'érgonomie. Disons que les intervenants principaux du projet ont chacun leur point d'importance : 
- **Le client** ne verra que le graphisme. Le projet a beau être d'une forte complexité et donc être une véritable prouesse technique, le client ne verra que le logo qui est trop petit à son gout. 
- **Le développeur** va accorder plus d'importance au code lui même : la qualité et à la performance sont ces objectifs maîtres. 

Trop souvent l'aspect ergonomique d'un projet est négligé et pourtant… Je vais donc m'appuyer sur un cas concret pour expliquer pourquoi le développeur doit aussi être un minimum ergonome. 

{% endexcerpt %}

## Une histoire d'inscription

Il ya quelques jours, j'ai décidé de m'inscrire pour un service disponible en ligne. Le service en question est un service professionnel et donc payant. Il est donc normal qu'un certain nombre d'informations me soient demandés à l'inscription comme mon nom, prénom, adresse, téléphone, etc. Jusque là, pas de problèmes, tout se passe bien. Je passe à l'étape suivante, je paie avec ma carte bleu et termine ma commande. 

A partir de la, les choses se gâtent. Je recois bien un email qui me dit que la commande a été reçue et puis… rien. Quelques heures passent. Toujours rien dans la boite mail. Je me connecte sur le site, aucune commande en attente, aucun service actif… Je commence à me poser des questions. Arnaque? Bug? Problème de paiement?

J'attends une journée entière, et je recois un mail du "Service validation" me demandant d'envoyer un scan de ma pièce d'identité et mon numéro de téléphone pour une validation de mon compte. Bien que je trouve la démarche étrange, je m'éxecute et je transmets donc les documents demandées. Puis encore une fois plus de nouvelles pendant 24h. 

Et c'est donc 48h après avoir passé ma commande que lors d'un échange avec le service client, j'apprends que "\[mon\] compte n'a pas passé les filtres anti-fraude" à cause d'un numéro de téléphone invalide. Et là, je réalise...

## L'importance de l'érgonomie 

En faute, c'est ce petit extrait du formulaire d'inscription : 

![Le champs d'inscription](/assets/screenshots/ergonomie-formulaire.png)

Il faut donc renseigner son numéro de téléphone. Personnellement, étant de nationalité belge et vivant en france, j'ai l'habitude de manipuler des numéros internationaux. Mon premier réflexe a donc été de renseigner le numéro de téléphone sans le 0 initial. 

Je cite ici [Wikipedia][wikipediatelephone] : 
>  \[…\] chaque numéro de téléphone possède deux formats, l'un lorsque l'appel reste interne au pays, l'autre dit international permettant d'appeler de n'importe quel pays.

Autrement dit dans le cas d'un numéro de téléphone francais, voici les deux formats :
- **06 xx xx xx xx** : format national que nous utilisons en temps normal
- **+33 6 xx xx xx xx** : le format international qui ajoute le préfixe du pays et qui fait sauter le zéro initial du numéro national.

Et c'est donc la que se trouve l'érreur érgonomique. En affichant l'indicatif international francais à coté du champs numéro de téléphone, j'ai cru devoir noter mon numéro de téléphone au format international ce qui n'était clairement pas le cas. De plus en validant, le formulaire, je n'ai eu aucun message m'indiquant que le format était invalide ou autre. 

## Show me the way

Pourtant les solutions pour éviter ce genre de problème sont à portée de main et facile à mettre en place. 

- **Mettre un message indicatif** : Une simple phrase en dessous du champs montrant le format attendu
- **Validation de la saisie** : Un warning de vérification à la validation du formulaire : *Votre numéro de téléphone semble être invalide, souhaitez-vous poursuivre?*
- **Utiliser un champs input HTML5 : phone** : Ce [nouveau type d'input][inputtel] permets de valider coté navigateur un numéro de téléphone et qui en plus à l'avantage de mieux fonctionner sur mobile.
- **Mettre le nom du pays plutot que l'indicatif**. 

Et je suis sûr qu'il en reste encore beaucoup d'autres... 

## Tout est bien qui finit bien

Pour terminer, je pense que c'est un bel exemple de manque d'érgonomie qui a des répercussions sur les clients. Même si aujourd'hui, j'ai bien accès au service, j'ai quand même perdu 48h, ponctués d'échanges pour régler un problème qui n'aurait jamais du apparaître. Et même si je pense que mon cas est isolé, il ne faut jamais négliger l'ergonomie dans le développement. 

[wikipediatelephone]: http://fr.wikipedia.org/wiki/Num%C3%A9ro_de_t%C3%A9l%C3%A9phone
[inputtel]: http://www.w3.org/TR/html-markup/input.tel.html
