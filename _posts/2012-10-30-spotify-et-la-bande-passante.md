---
layout: post
category: article 
title: Spotify et la bande passante
---

# Spotify et la bande passante 

Petit post pour parler d'un truc que j'ai découvert aujourd'hui et que je trouve assez hallucinant. Je suis un grand utilisateur de [Spotify][spotify]. Je me suis abonné en compte premium, je m'en sers sur toutes mes machines ainsi que sur mon téléphone portable. Toujours très satisfait de la qualité, du choix tout ca, mais le débat n'est pas la. J'ai depuis peu une connection internet lamentable. Du genre vraiment lamentable, même la télévision via la box ne marche pas, "pas assez de débit". 

Du coup, je commence à faire attention à ce que je consomme en bande passante, je lance les grosses mises-à-jour la nuit, etc. Mais il restait une inconnue dans ce système. Comment se comporte Spotify dans l'histoire? 

## Un modèle P2P

Et oui, Spotify fonctionne en peer-2-peer. Je n'en avais absolument aucune idée mais en y reflechissant, ça me parait logique. Si ils devaient heberger tout le contenu eux-même et le "streamer" vers chaque utilisateur, il leur faudrait une infrastructure proche de celle de youtube et donc des frais très importants et par conséquent une viabilité économique mise en cause. Du coup, c'est le choix du p2p qui a été fait. Et le résultat est la. L'objectif était [une latence de 285 milliseconds][285ms] entre le clic et le début de la lecture. C'est d'ailleurs [un ancien dirigeant de Limewire][limewire-linkedin] qui travaille aujourd'hui sur la partie p2p chez Spotify. 

La différence avec les autres systèmes de peer-2-peer, c'est que sur Spotify il est impossible de désactiver la partie seeding. Quand vous écoutez une chanson sur Spotify, le titre est stocké dans le cache de Spotify. Ensuite un index est construit à partir des morceaux qui sont stockés dans le cache et cet index est ensuite envoyés sur les serveurs de Spotify. Et quand quelqu'un demande une chanson qui se trouve dans le cache, il se peut très bien que l'ordinateur devienne donc une source pour les autres utilisateurs. 

## Les chiffres

Rien d'officiel sur ce coté la, mais après une bonne heure ou deux de surveillance, je constate : 

1. Le streaming sortant est sporadique. (1 fois toutes les 2,3 minutes)
2. De relativement courte durée (entre 10 et 30 secondes)
3. Le débit sortant n'est pas important mais significatif dans mon cas. (50ko/s sortant)
4. Les transferts sont constants, aussi bien en durée que en vitesse.

## Les solutions

Certains utilisateurs ont cherché des solutions sans vraiment en trouver. Une première solution est de filtrer le traffic reseau. C'est à dire bloquer toutes les connections reseau de spotify puis lui autoriser une connection uniquement sur les serveurs de spotify. De cette facon la, les connections venant d'autres utilisateurs sont bloqués. 

L'alternative que j'ai choisi, c'est l'utilisation de l'application mobile. En effet, l'application mobile ne focntionne pas en p2p mais en connection directe avec le serveur. Ca me permets donc d'épargner un peu ma (petite) connection. 

PS : Il y a pas mal d'informations sur la page [Wikipedia][wikipedia] et sur dans article : [How Spotify works][npr-article]

[spotify]: http://www.spotify.com/
[285ms]: http://www.spotify.com/fr/jobs/view/ocnMWfw1/
[wikipedia]: http://en.wikipedia.org/wiki/Spotify
[npr-article]: http://www.npr.org/blogs/therecord/2011/11/09/141594727/how-spotify-works-pay-the-majors-use-p2p-technology
[limewire-linkedin]: http://www.linkedin.com/in/jpavley