# Travailler avec les listes

:::tip
Les listes sont équivalentes aux colonnes individuelles des tableaux. Vous pouvez penser à une liste comme étant essentiellement une « table à une colonne » (sans nom de colonne). Ainsi, toute commande qui opère sur une colonne _fonctionne également_ sur une liste. Par exemple, [`where`](/commands/docs/where.md) peut être utilisé avec les listes :

```nu
[bell book candle] | where ($it =~ 'b')
# => ╭───┬──────╮
# => │ 0 │ bell │
# => │ 1 │ book │
# => ╰───┴──────╯
```

:::

## Création de listes

Une liste est une collection ordonnée de valeurs.
Une liste est créée en utilisant des crochets carrés entourant des valeurs séparées par des espaces, des sauts de ligne et/ou des virgules.
Par exemple, `[foo bar baz]` ou `[foo, bar, baz]`.

::: tip
Les listes Nushell sont similaires aux tableaux JSON. Le même `[ "Item1", "Item2", "Item3" ]` qui représente un tableau JSON peut également être utilisé pour créer une liste Nushell.
:::

## Mise à jour des listes

Nous pouvons [`insert`](/commands/docs/insert.md) des valeurs dans les listes au fur et à mesure qu'elles circulent dans le pipeline, par exemple, insérons la valeur `10` au milieu d'une liste :

```nu
[1, 2, 3, 4] | insert 2 10
# => [1, 2, 10, 3, 4]
```

Nous pouvons également utiliser [`update`](/commands/docs/update.md) pour remplacer le 2e élément par la valeur `10`.

```nu
[1, 2, 3, 4] | update 1 10
# => [1, 10, 3, 4]
```

## Suppression ou ajout d'éléments à la liste

En plus de [`insert`](/commands/docs/insert.md) et [`update`](/commands/docs/update.md), nous avons également [`prepend`](/commands/docs/prepend.md) et [`append`](/commands/docs/append.md). Celles-ci vous permettent d'insérer au début d'une liste ou à la fin de la liste, respectivement.

Par exemple :

```nu
let colors = [yellow green]
let colors = ($colors | prepend red)
```
