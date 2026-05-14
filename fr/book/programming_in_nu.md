---
prev:
  text: Variables spéciales
  link: /book/special_variables.md
next:
  text: Commandes personnalisées
  link: /book/custom_commands.md
---

# Programmation en Nu

Ce chapitre entre dans plus de détails de Nushell en tant que langage de programmation.
Chaque fonctionnalité majeure du langage a sa propre section.

Tout comme la plupart des langages de programmation vous permettent de définir des fonctions, Nushell utilise des [commandes personnalisées](custom_commands.md) à cette fin.

D'autres shells pourraient vous être familiers avec les [alias](aliases.md).
Les alias de Nushell fonctionnent de la même manière et sont une partie du langage de programmation, et non seulement une fonctionnalité de shell.

Les opérations courantes, telles que l'addition ou la recherche regex, peuvent être effectuées avec des [opérateurs](operators.md).
Toutes les opérations ne sont pas supportées pour tous les types de données, et Nushell s'assurera de vous laisser savoir quand il y a une non-correspondance.

Vous pouvez stocker les résultats intermédiaires dans des [variables](variables.md).
Les variables peuvent être immuables, mutables ou une constante de temps d'analyse.

Les trois dernières sections visent à organiser votre code :

Les [scripts](scripts.md) sont la forme la plus simple d'organisation de code : vous mettez simplement le code dans un fichier et le sourcez.
Cependant, vous pouvez également exécuter des scripts comme des programmes autonomes avec des signatures de ligne de commande en utilisant la commande « spéciale » `main`.

Avec les [modules](modules.md), tout comme dans de nombreux autres langages de programmation, il est possible de composer votre code à partir de pièces plus petites.
Les modules vous permettent de définir une interface publique par rapport à des commandes privées et vous pouvez importer des commandes personnalisées, des alias et des variables d'environnement à partir d'elles.

Les [overlays](overlays.md) sont basés sur les modules.
En définissant un overlay, vous apportez les définitions du module dans sa propre « couche » échangeable qui s'applique au-dessus d'autres overlays.
Cela permet des fonctionnalités comme l'activation d'environnements virtuels ou la substitution d'ensembles de commandes par défaut par des variantes personnalisées.

La bibliothèque standard a également un [cadre de test](testing.md) si vous voulez prouver que votre code réutilisable fonctionne parfaitement.
