---
layout: post
title: L'annotation Template dans Symfony2
groups: "articles"
---

# L'annotation Template dans Symfony2

Premier billet sur Symfony2. Et pour commencer un premier conseil/astuce assez pratique. J'ai découvert hier l'annotation **@Template()**. 
Comme nous l'indique la [documentation de Symfony2](http://symfony.com/doc/2.0/bundles/SensioFrameworkExtraBundle/annotations/view.html), cette annotation fait partie des bundles *Extra* de symfony qui propose tout un tas de petites choses qui font que Symfony se démarque des autres framework PHP. Mais revenons en à Template. 

Quand je code en PHP (ou mon équipe d'ailleurs), j'essaye toujours de me donner des conventions de nommage et de les respecter. Dans la partie MVC avec Symfony par exemple, le nom du controlleur sera explicite du Type **AdminController** contenant des noms méthodes aux noms explicites également. Ce qui donne un code de ce style là : 


<div class="syntax">
{% hightlight php %}
class AdminController extends Controller {

	public function indexAction() { .. }
	public function updateAction() { ... }
		
}	
{% endhightlight %}
</div>

Pour rester cohérent jusqu'au bout, on essaye aussi de garder les mêmes noms entre les vues et les noms des fonctions. Du coup dans notre dossier **views**, on aura : 

<div class="syntax">
{% hightlight  %}
 - views
 |   - admin
 |   |	- index.html.twig
 |   |  - update.html.twig
{% endhighlight %}
</div>


Pour faire simple, **@Template()** va chercher la vue portant le nom de la fonction dans le dossier portant le nom du controlleur et l'associer automatiquement à la fonction. Plus besoin de faire :

<pre class="prettyprint">
	return $this->render('MonBundle:moncontrolleur:mavue.html.twig', $mesparams);
</pre>

Un simple return des paramètres passés à la vue fera l'affaire. De plus, l'annotation **@Template** force une cohérence entre nom de controlleur, noms de fonction et noms de vues. Tout simplement si, cette convention de nommage n'est pas respecté, le code ne pourra s'éxécuter et donc erreur 500. Une bonne pratique sera donc d'utiliser l'annotation @Template autant que possible.

PS : Le seul regret que je puisse exprimer vis à vis de cette annotation est qu'elle se limite à une fonction et qu'elle n'est pas applicable globalement sur un controller.  