---
prev:
  text: Programmation en Nu
  link: /book/programming_in_nu.md
---

# Commandes personnalisées

Comme avec n'importe quel langage de programmation, vous allez rapidement vouloir sauvegarder des pipelines plus longs et des expressions afin que vous puissiez les appeler à nouveau facilement quand vous en avez besoin.

C'est là que les commandes personnalisées entrent en jeu.

::: tip Remarque
Les commandes personnalisées sont similaires aux fonctions dans de nombreux langages, mais dans Nushell, les commandes personnalisées _agissent comme des commandes de première classe elles-mêmes_. Comme vous le verrez ci-dessous, elles sont incluses dans le système d'aide aux côté des commandes intégrées, peuvent faire partie d'un pipeline, sont analysées en temps réel pour les erreurs de type, et bien plus.
:::

[[toc]]

## Créer et exécuter une commande personnalisée

Commençons avec une commande personnalisée simple `greet` :

```nu
def greet [name] {
  $"Hello, ($name)!"
}
```

Ici, nous définissons la commande `greet`, qui prend un seul paramètre `name`. Après ce paramètre se trouve le bloc qui représente ce qui se passera quand la commande personnalisée s'exécute. Quand elle est appelée, la commande personnalisée définira la valeur passée pour `name` comme la variable `$name`, qui sera disponible pour le bloc.

Pour exécuter cette commande, nous pouvons l'appeler tout comme nous appellerions les commandes intégrées :

```nu
greet "World"
# => Hello, World!
```

## Retourner des valeurs des commandes

Vous remarquerez peut-être qu'il n'y a pas d'instruction `return` ou `echo` dans l'exemple ci-dessus.

Comme certains autres langages, tels que PowerShell et JavaScript (avec les fonctions fléchées), Nushell propose un _retour implicite_, où la valeur de l'expression finale dans la commande devient sa valeur de retour.

Dans l'exemple ci-dessus, il n'y a qu'une seule expression — la chaîne. Cette chaîne devient la valeur de retour de la commande.

```nu
greet "World" | describe
# => string
```

Une commande typique, bien sûr, sera composée de plusieurs expressions. À titre d'exemple, voici une commande insensée qui a 3 expressions :

```nu
def eight [] {
  1 + 1
  2 + 2
  4 + 4
}

eight
# => 8
```

La valeur de retour, encore une fois, est simplement le résultat de l'expression _finale_ dans la commande, qui est `4 + 4` (8).

Exemples supplémentaires :

::: details Retour précoce
Les commandes qui doivent quitter plus tôt en raison d'une condition peuvent toujours retourner une valeur en utilisant l'[instruction `return`](/commands/docs/return.md).

```nu
def process-list [] {
  let input_length = length
  if $input_length > 10_000 {
    print "Input list is too long"
    return null
  }

  $in | each {|i|
    # Process the list
    $i * 4.25
  }
}
```

:::

::: details Suppression de la valeur de retour
Vous voudrez souvent créer une commande personnalisée qui agit comme une _instruction_ plutôt qu'une expression et ne retourne pas de valeur.

Vous pouvez utiliser le mot-clé `ignore` dans ce cas :

```nu
def create-three-files [] {
  [ file1 file2 file3 ] | each {|filename|
    touch $filename
  } | ignore
}
```
