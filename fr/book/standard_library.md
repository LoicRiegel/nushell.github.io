---
prev:
  text: (Pas si) Avancé
  link: /book/advanced.md
---

# Bibliothèque standard (Aperçu)

Nushell est livré avec une bibliothèque standard de commandes utiles écrites en Nu natif. Par défaut, la bibliothèque standard est chargée en mémoire (mais pas automatiquement importée) au démarrage de Nushell.

[[toc]]

## Aperçu

La bibliothèque standard inclut actuellement :

- Assertions
- Un système `help` alternatif avec support des complétions.
- Formats de variantes JSON supplémentaires
- Accès XML
- Journalisation
- Et plus

Pour voir une liste complète des commandes disponibles dans la bibliothèque standard, exécutez ce qui suit :

```nu
nu -c "
  use std
  scope commands
  | where name =~ '^std '
  | select name description extra_description
  | wrap 'Standard Library Commands'
  | table -e
"
```

::: note
La commande `use std` ci-dessus charge la bibliothèque standard complète afin que vous puissiez voir toutes les commandes à la fois. Ce n'est généralement pas ainsi qu'elle sera utilisée (plus d'informations ci-dessous). Elle est également exécutée dans un sous-shell Nu séparé simplement afin qu'elle ne soit pas chargée dans la scope du shell que vous utilisez.
:::

## Importation de la bibliothèque standard

Les modules et sous-modules de la bibliothèque standard sont importés avec la commande [`use`](/commands/docs/use.md), tout comme n'importe quel autre module. Voir [Utilisation de modules](./modules/using_modules.md) pour plus d'informations.

Lors du travail à la ligne de commande, il peut être pratique de charger la bibliothèque standard complète en utilisant :

```nu
use std *
```

Cependant, cette forme doit être évitée dans les commandes personnalisées et les scripts car elle a le plus long temps de chargement.
