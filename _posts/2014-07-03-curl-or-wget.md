---
layout: post
category: article
title: CURL ou WGET    
comments: true
---

# CURL ou WGET ?

Ce sont tout les deux des utilitaires qu'on utilise presque quotidiennement dans le monde du développement et pourtant j'ai jamais su faire la différence entre les deux. Les deux permettent de récupérer des fichiers en ligne de commande. J'ai décidé de creuser un peu et de rassembler mes notes ici. 

## cUrl 

Tout d'abord, ce n'est pas seulement la ligne de commande. Comme le montre wikipedia, curl est composé de deux choses :
- cUrl : une ligne de commande pour l'utilisation de la librairie libcurl
- libcurl : une librairie avec toutes les fonctionnalités 


## wget



## Code source

Juste par curiosité, un petit coup d'oeil sur le code source : 

![CLOC Curl](assets/images/cloc-curl.png)
![CLOC WGET](assets/images/cloc-wget.png)


## Utilisation basique

Les deux s'utilise en ligne de commande et ont une syntaxe relativement proche. La première différence qui saute aux yeux : pour une requête http, wget sauvegarde le fichier avec le nom de la ressource distante, tandis que curl va rediriger le fichier sur la sortie standard. Ce qui oblige à soit utiliser une redirection dans un fichier, soit de préciser un paramètre supplémentaire. 

    wget http://test-debit.free.fr/512.rnd
    curl -O http://test-debit.free.fr/512.rnd 
    
## Options
