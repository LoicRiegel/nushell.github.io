# Flux de contrôle

Nushell fournit plusieurs commandes qui aident à déterminer comment les différents groupes de code sont exécutés. Dans les langages de programmation, cette fonctionnalité est souvent appelée _flux de contrôle_.

::: tip
Une chose à noter est que toutes les commandes discutées sur cette page utilisent des [blocs](/book/types_of_data.html#blocks). Cela signifie que vous pouvez muter des [variables d'environnement](/book/environment.html) et autres [variables mutable](/book/variables.html#mutable-variables) en eux.
:::

## Déjà couvert

Ci-dessous, nous couvrons certaines commandes liées au flux de contrôle, mais avant d'y arriver, il est utile de noter qu'il existe plusieurs fonctionnalités et concepts qui ont déjà été couverts dans d'autres sections et qui sont également liés au flux de contrôle ou qui peuvent être utilisés dans les mêmes situations. Ceux-ci incluent :

- Les pipelines sur la page [pipelines](/book/pipelines.html).
- Les fermetures sur la page [types de donnée](/book/types_of_data.html).
- Commandes d'itération sur la page [travailler avec des listes](/book/working_with_lists.html). Tel que :
  - [`each`](/commands/docs/each.html)
  - [`where`](/commands/docs/where.html)
  - [`reduce`](/commands/docs/reduce.html)

## Choix (Conditionnels)

Les commandes suivantes exécutent du code basé sur une condition donnée.

::: tip
Les commandes de choix/conditionnelles sont des expressions donc elles retournent des valeurs, contrairement aux autres commandes de cette page. Cela signifie que ce qui suit fonctionne.

```nu
'foo' | if $in == 'foo' { 1 } else { 0 } | $in + 2
# => 3
```

:::

### `if`

[`if`](/commands/docs/if.html) évalue les [blocs](/book/types_of_data.html#blocks) de code branchants basés sur les résultats d'une ou plusieurs conditions similaires à la fonctionnalité « if » dans d'autres langages de programmation. Par exemple :

```nu
if $x > 0 { 'positive' }
```

Retourne `'positive'` lorsque la condition est `true` (`$x` est supérieur à zéro) et `null` lorsque la condition est `false` (`$x` est inférieur ou égal à zéro).

Nous pouvons ajouter une branche `else` à la `if` après le premier bloc qui exécute et retourne la valeur résultante du bloc `else` quand la condition est `false`. Par exemple :

```nu
if $x > 0 { 'positive' } else { 'non-positive' }
```

Cette fois, il retourne `'positive'` lorsque la condition est `true` (`$x` est supérieur à zéro) et `'non-positive'` lorsque la condition est `false` (`$x` est inférieur ou égal à zéro).

Nous pouvons aussi chaîner plusieurs `if` ensemble comme suit :

```nu
if $x > 0 { 'positive' } else if $x == 0 { 'zero' } else { "negative" }
```

Quand la première condition est `true` (`$x` est supérieur à zéro), il retournera `'positive'`, quand la première condition est `false` et la condition suivante est `true` (`$x` est égal à zéro), il retournera `'zero'`, sinon il retournera `'negative'` (quand `$x` est inférieur à zéro).

### `match`

[`match`](/commands/docs/match.html) exécute une de plusieurs branches conditionnelles basées sur la valeur donnée pour correspondre. Vous pouvez aussi faire de la [correspondance de motif](/cookbook/pattern_matching.html) pour déballer les valeurs dans les types composites comme les listes et les enregistrements.

L'utilisation de base de [`match`](/commands/docs/match.html) peut exécuter conditionnellement un code différent comme une instruction « switch » commune dans d'autres langages. [`match`](/commands/docs/match.html) vérifie si la valeur après le mot [`match`](/commands/docs/match.html) est égale à la valeur au début de chaque branche avant le `=>` et si c'est le cas, il exécute le code après le `=>` de cette branche.

```nu
match 3 {
    1 => 'one',
    2 => {
        let w = 'w'
        't' + $w + 'o'
    },
    3 => 'three',
    4 => 'four'
}
# => three
```

Les branches peuvent soit retourner une valeur unique, soit, comme montré dans la deuxième branche, peuvent retourner les résultats d'un [bloc](/book/types_of_data.html#blocks).

#### Branche fourre-tout

Vous pouvez aussi avoir une condition de fourre-tout pour quand la valeur donnée ne correspond à aucune des autres conditions en ayant une branche dont la valeur correspondante est `_`.

```nu
let foo = match 7 {
    1 => 'one',
    2 => 'two',
    3 => 'three',
    _ => 'other number'
}
$foo
# => other number
```

(Rappel, [`match`](/commands/docs/match.html) est une expression, c'est pourquoi nous pouvons assigner le résultat à `$foo` ici).

#### Correspondance de motif

Vous pouvez « déballer » des valeurs à partir de types comme des listes et des enregistrements avec la [correspondance de motif](/cookbook/pattern_matching.html). Vous pouvez ensuite assigner des variables aux parties que vous voulez déballer et les utiliser dans les expressions correspondantes.
