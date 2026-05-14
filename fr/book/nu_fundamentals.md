---
prev:
  text: Antisèche Nushell
  link: cheat_sheet.md
next:
  text: Types de données
  link: /book/types_of_data.md
---

# Fondamentaux de Nushell

Ce chapitre explique certains des fondamentaux du langage de programmation Nushell.
Après l'avoir parcouru, vous devriez avoir une idée de comment écrire de simples programmes Nushell.

Nushell a un système de type riche.
Vous trouverez des types de données typiques tels que les chaînes ou les entiers et des types de données moins typiques, tels que les chemins de cellule.
De plus, l'une des caractéristiques définissantes de Nushell est la notion de _données structurées_ ce qui signifie que vous pouvez organiser les types en collections : listes, enregistrements ou tableaux.
Contrairement à l'approche Unix traditionnelle où les commandes communiquent par du texte brut, les commandes Nushell communiquent via ces types de données.
Tout ce qui précède est expliqué dans [Types de données](types_of_data.md).

[Chargement de données](loading_data.md) explique comment lire les formats de données courants, comme JSON, dans les _données structurées_. Cela inclut notre propre format de données « NUON ».

Comme dans les shells Unix, les commandes Nushell peuvent être composées en [pipelines](pipelines.md) pour transmettre et modifier un flux de données.

Certains types de données ont des fonctionnalités intéressantes qui méritent leurs propres sections : [chaînes](working_with_strings.md), [listes](working_with_lists.md) et [tableaux](working_with_tables.md).
En plus d'expliquer les fonctionnalités, ces sections montrent également comment effectuer certaines opérations courantes, telles que la composition de chaînes ou la mise à jour de valeurs dans une liste.

Enfin, [Référence de commande](/commands/) liste toutes les commandes intégrées avec de brèves descriptions.
Notez que vous pouvez également accéder à ces informations depuis Nushell en utilisant la commande [`help`](/commands/docs/help.md).
