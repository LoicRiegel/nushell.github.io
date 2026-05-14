# Travailler avec les tableaux

[[toc]]

## Aperçu

L'une des façons courantes de voir les données dans Nu est through un tableau. Nu vient avec un certain nombre de commandes pour travailler avec les tableaux afin de le rendre pratique de trouver ce que vous cherchez, et pour réduire les données à seulement ce dont vous avez besoin.

Pour commencer, prenons un tableau avec lequel nous pouvons travailler :

```nu
ls
# => ───┬───────────────┬──────┬─────────┬────────────
# =>  # │ name          │ type │ size    │ modified
# => ───┼───────────────┼──────┼─────────┼────────────
# =>  0 │ files.rs      │ File │  4.6 KB │ 5 days ago
# =>  1 │ lib.rs        │ File │   330 B │ 5 days ago
# =>  2 │ lite_parse.rs │ File │  6.3 KB │ 5 days ago
# =>  3 │ parse.rs      │ File │ 49.8 KB │ 1 day ago
# =>  4 │ path.rs       │ File │  2.1 KB │ 5 days ago
# =>  5 │ shapes.rs     │ File │  4.7 KB │ 5 days ago
# =>  6 │ signature.rs  │ File │  1.2 KB │ 5 days ago
# => ───┴───────────────┴──────┴─────────┴────────────
```

::: tip Modification de l'affichage des tableaux
Nu essaiera de développer la structure complète du tableau par défaut. Vous pouvez modifier ce comportement en modifiant le hook `display_output`.
Voir [hooks](/book/hooks.md#changing-how-output-is-displayed) pour plus d'informations.
:::

## Tri des données

Nous pouvons trier un tableau en appelant la commande [`sort-by`](/commands/docs/sort-by.md) et en lui indiquant les colonnes que nous voulons utiliser dans le tri. Disons que nous voulions trier notre tableau par la taille du fichier :

```nu
ls | sort-by size
# => ───┬───────────────┬──────┬─────────┬────────────
# =>  # │ name          │ type │ size    │ modified
# => ───┼───────────────┼──────┼─────────┼────────────
# =>  0 │ lib.rs        │ File │   330 B │ 5 days ago
# =>  1 │ signature.rs  │ File │  1.2 KB │ 5 days ago
# =>  2 │ path.rs       │ File │  2.1 KB │ 5 days ago
# =>  3 │ files.rs      │ File │  4.6 KB │ 5 days ago
# =>  4 │ shapes.rs     │ File │  4.7 KB │ 5 days ago
# =>  5 │ lite_parse.rs │ File │  6.3 KB │ 5 days ago
# =>  6 │ parse.rs      │ File │ 49.8 KB │ 1 day ago
# => ───┴───────────────┴──────┴─────────┴────────────
```

Nous pouvons trier un tableau par n'importe quelle colonne qui peut être comparée. Par exemple, nous aurions également pu trier ce qui précède en utilisant les colonnes « name », « accessed » ou « modified ».
