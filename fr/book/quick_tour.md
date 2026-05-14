---
prev:
  text: Getting Started
  link: /book/getting_started.md
---

# Rapide Tour d'Horizon

[[toc]]

## Les Commandes Nushell Produisent des _Données_

La façon la plus simple de découvrir ce que Nu peut faire est de commencer par quelques exemples, alors plongeons-y.

La première chose que vous remarquerez en exécutant une commande comme [`ls`](/commands/docs/ls.md) est qu'au lieu de recevoir un bloc de texte, vous obtenez un tableau structuré.

```nu:no-line-numbers
ls
# => ╭────┬─────────────────────┬──────┬───────────┬──────────────╮
# => │  # │        name         │ type │   size    │   modified   │
# => ├────┼─────────────────────┼──────┼───────────┼──────────────┤
# => │  0 │ CITATION.cff        │ file │     812 B │ 2 months ago │
# => │  1 │ CODE_OF_CONDUCT.md  │ file │   3.4 KiB │ 9 months ago │
# => │  2 │ CONTRIBUTING.md     │ file │  11.0 KiB │ 5 months ago │
# => │  3 │ Cargo.lock          │ file │ 194.9 KiB │ 15 hours ago │
# => │  4 │ Cargo.toml          │ file │   9.2 KiB │ 15 hours ago │
# => │  5 │ Cross.toml          │ file │     666 B │ 6 months ago │
# => │  6 │ LICENSE             │ file │   1.1 KiB │ 9 months ago │
# => │  7 │ README.md           │ file │  12.0 KiB │ 15 hours ago │
# => ...
```

Ce tableau fait plus que simplement afficher la sortie joliment. Comme un tableur, il nous permet de travailler avec les données de façon _interactive_.

## Agir sur les Données

Trions maintenant ce tableau selon la taille de chaque fichier. Pour cela, nous allons prendre la sortie de [`ls`](/commands/docs/ls.md) et la passer à une commande capable de trier les tableaux selon les _valeurs_ d'une colonne.

```nu:no-line-numbers
ls | sort-by size | reverse
# => ╭───┬─────────────────┬──────┬───────────┬──────────────╮
# => │ # │      name       │ type │   size    │   modified   │
# => ├───┼─────────────────┼──────┼───────────┼──────────────┤
# => │ 0 │ Cargo.lock      │ file │ 194.9 KiB │ 15 hours ago │
# => │ 1 │ toolkit.nu      │ file │  20.0 KiB │ 15 hours ago │
# => │ 2 │ README.md       │ file │  12.0 KiB │ 15 hours ago │
# => │ 3 │ CONTRIBUTING.md │ file │  11.0 KiB │ 5 months ago │
# => │ 4 │ ...             │ ...  │ ...       │ ...          │
# => │ 5 │ LICENSE         │ file │   1.1 KiB │ 9 months ago │
# => │ 6 │ CITATION.cff    │ file │     812 B │ 2 months ago │
# => │ 7 │ Cross.toml      │ file │     666 B │ 6 months ago │
# => │ 8 │ typos.toml      │ file │     513 B │ 2 months ago │
# => ╰───┴─────────────────┴──────┴───────────┴──────────────╯
```

Remarquez que nous n'avons pas passé d'arguments ni de flags à [`ls`](/commands/docs/ls.md). Au lieu de cela, nous avons utilisé la commande intégrée [`sort-by`](/commands/docs/sort-by.md) de Nushell pour trier la _sortie_ de la commande `ls`. Puis, pour voir les fichiers les plus volumineux en haut, nous avons appliqué [`reverse`](/commands/docs/reverse.md) sur la _sortie_ de `sort-by`.

::: tip Cool !
Si vous comparez attentivement l'ordre de tri, vous remarquerez peut-être que les données ne sont pas triées alphabétiquement, ni même par valeurs _numériques_. En effet, puisque la colonne `size` est de type [`filesize`](./types_of_data.md#file-sizes), Nushell sait que `1.1 KiB` (kibioctets) est plus grand que `812 B` (octets).
:::

# Rechercher des Données avec la Commande `where`

Nu propose de nombreuses commandes pouvant agir sur la sortie structurée de la commande précédente. Celles-ci sont généralement classées comme « Filtres » dans Nushell.

Par exemple, nous pouvons utiliser [`where`](/commands/docs/where.md) pour filtrer le tableau afin de n'afficher que les fichiers de plus de 10 kilooctets :

```nu
ls | where size > 10kb
# => ╭───┬─────────────────┬──────┬───────────┬───────────────╮
# => │ # │      name       │ type │   size    │   modified    │
# => ├───┼─────────────────┼──────┼───────────┼───────────────┤
# => │ 0 │ CONTRIBUTING.md │ file │  11.0 KiB │ 5 months ago  │
# => │ 1 │ Cargo.lock      │ file │ 194.6 KiB │ 2 minutes ago │
# => │ 2 │ README.md       │ file │  12.0 KiB │ 16 hours ago  │
# => │ 3 │ toolkit.nu      │ file │  20.0 KiB │ 16 hours ago  │
# => ╰───┴─────────────────┴──────┴───────────┴───────────────╯
```

## Bien Plus Que des Répertoires

Bien entendu, cela ne se limite pas à la commande `ls`. Nushell suit la philosophie Unix où chaque commande fait bien une seule chose, et la sortie d'une commande devient généralement l'entrée d'une autre. Cela nous permet de combiner les commandes de nombreuses façons différentes.

Voyons une autre commande :

```nu:no-line-numbers
ps
# => ╭───┬──────┬──────┬───────────────┬──────────┬──────┬───────────┬─────────╮
# => │ # │ pid  │ ppid │     name      │  status  │ cpu  │    mem    │ virtual │
# => ├───┼──────┼──────┼───────────────┼──────────┼──────┼───────────┼─────────┤
# => │ 0 │    1 │    0 │ init(void)    │ Sleeping │ 0.00 │   1.2 MiB │ 2.2 MiB │
# => │ 1 │    8 │    1 │ init          │ Sleeping │ 0.00 │ 124.0 KiB │ 2.3 MiB │
# => │ 2 │ 6565 │    1 │ SessionLeader │ Sleeping │ 0.00 │ 108.0 KiB │ 2.2 MiB │
# => │ 3 │ 6566 │ 6565 │ Relay(6567)   │ Sleeping │ 0.00 │ 116.0 KiB │ 2.2 MiB │
# => │ 4 │ 6567 │ 6566 │ nu            │ Running  │ 0.00 │  28.4 MiB │ 1.1 GiB │
# => ╰───┴──────┴──────┴───────────────┴──────────┴──────┴───────────┴─────────╯
```

Vous connaissez peut-être la commande `ps` de Linux/Unix. Elle fournit la liste de tous les processus en cours d'exécution sur le système ainsi que leur état. Comme pour `ls`, Nushell propose une commande [`ps`](/commands/docs/ps.md) intégrée et multiplateforme qui renvoie ses résultats sous forme de données structurées.

::: note
La commande `ps` Unix traditionnelle n'affiche par défaut que le processus courant et ses parents. L'implémentation de Nushell affiche par défaut tous les processus du système.

Normalement, exécuter `ps` dans Nushell utilise sa commande **_interne_** multiplateforme. Il est toutefois possible d'exécuter la version **_externe_** dépendante du système sur les plateformes Unix/Linux en la préfixant du _caret sigil_. Par exemple :

```nu
^ps aux  # exécute la commande Unix ps avec tous les processus en format orienté utilisateur
```

Voir [Exécuter des Commandes Système Externes](./running_externals.md) pour plus de détails.
:::

Et si nous voulions afficher uniquement les processus en cours d'exécution ? Comme avec `ls`, nous pouvons aussi travailler avec le tableau que `ps` _produit_ :

```nu
ps | where status == Running
# => ╭───┬──────┬──────┬──────┬─────────┬──────┬──────────┬─────────╮
# => │ # │ pid  │ ppid │ name │ status  │ cpu  │   mem    │ virtual │
# => ├───┼──────┼──────┼──────┼─────────┼──────┼──────────┼─────────┤
# => │ 0 │ 6585 │ 6584 │ nu   │ Running │ 0.00 │ 31.9 MiB │ 1.2 GiB │
# => ╰───┴──────┴──────┴──────┴─────────┴──────┴──────────┴─────────╯
```

::: tip
Souvenez-vous que la colonne `size` de la commande `ls` était de type `filesize` ? Ici, `status` est simplement une string, et vous pouvez utiliser toutes les opérations et commandes de string habituelles, y compris (comme ci-dessus) la comparaison `==`.

Vous pouvez examiner les types des colonnes d'un tableau avec :

```nu
ps | describe
# => table<pid: int, ppid: int, name: string, status: string, cpu: float, mem: filesize, virtual: filesize> (stream)
```

La [commande `describe`](/commands/docs/describe.md) permet d'afficher le type de sortie de n'importe quelle commande ou expression.
:::

## Arguments de Commande dans un Pipeline

Parfois, une commande prend un _argument_ plutôt qu'une _entrée_ de pipeline. Pour ce cas, Nushell fournit la [variable `$in`](./pipelines.md#pipeline-input-and-the-special-in-variable) qui vous permet d'utiliser la sortie de la commande précédente sous forme de variable. Par exemple :

```nu:line-numbers
ls
| sort-by size
| reverse
| first
| get name
| cp $in ~
```

::: tip Note de Conception Nushell
Dans la mesure du possible, les commandes Nushell sont conçues pour agir sur l'_entrée_ du pipeline. Toutefois, certaines commandes, comme `cp` dans cet exemple, ont deux (ou plusieurs) arguments de significations différentes. Dans ce cas, `cp` doit connaître à la fois le chemin à _copier_ et le chemin de _destination_. Cette commande est donc plus ergonomique avec deux _paramètres positionnels_.
:::

::: tip
Les commandes Nushell peuvent s'étendre sur plusieurs lignes pour améliorer la lisibilité. Ce qui précède est équivalent à :

```nu
ls | sort-by size | reverse | first | get name | cp $in ~
```

Voir aussi : [Édition Multi-lignes](./line_editor.md#multi-line-editing)
:::

Les trois premières lignes sont les mêmes commandes utilisées dans le deuxième exemple, examinons les trois dernières :

4. La [commande `first`](/commands/docs/first.md) retourne simplement la première valeur du tableau. Ici, cela correspond au fichier de plus grande taille, soit `Cargo.lock` si l'on utilise le listing du deuxième exemple. Ce « fichier » est un [`record`](./types_of_data.md#records) du tableau qui contient toujours ses colonnes/champs `name`, `type`, `size` et `modified`.
5. `get name` retourne la _valeur_ du champ `name` de la commande précédente, soit `"Cargo.lock"` (une string). C'est aussi un exemple simple de [`cell-path`](./types_of_data.md#cell-paths) utilisable pour naviguer dans des données structurées.
6. La dernière ligne utilise la variable `$in` pour référencer la sortie de la ligne 5. Le résultat est une commande qui signifie _« Copier 'Cargo.lock' dans le répertoire home »_

::: tip
[`get`](/commands/docs/get.md) et son équivalent [`select`](/commands/docs/select.md) sont deux des filtres les plus utilisés dans Nushell, mais la différence entre eux n'est pas toujours évidente au premier abord. Quand vous êtes prêt à les utiliser plus intensivement, consultez [Utiliser `get` et `select`](./navigating_structured_data.md#using-get-and-select).
:::

## Obtenir de l'Aide

Nushell dispose d'un système d'aide intégré et complet. Par exemple :

```nu
# help <commande>
help ls
# Ou
ls --help
# Également
help operators
help escapes
```

::: tip Cool !
Appuyez sur la touche <kbd>F1</kbd> pour accéder au [menu](./line_editor.md#menus) d'aide. Recherchez la commande `ps`, mais _ne pressez pas encore <kbd>Entrée</kbd>_ !

Appuyez plutôt sur la touche <kbd>Flèche vers le bas</kbd> et remarquez que vous faites défiler la section Exemples. Sélectionnez un exemple, _puis_ appuyez sur <kbd>Entrée</kbd> et l'exemple sera saisi dans la ligne de commande, prêt à être exécuté !

C'est une excellente façon d'explorer et d'apprendre l'ensemble étendu des commandes Nushell.
:::

Le système d'aide dispose également d'une fonctionnalité de « recherche » :

```nu
help --find filesize
# ou
help -f filesize
```

Vous ne serez peut-être pas surpris d'apprendre que le système d'aide est lui-même basé sur des données structurées ! Notez que la sortie de `help -f filesize` est un tableau.

L'aide de chaque commande est stockée sous forme de record contenant :

- Le nom
- La catégorie
- Le type (intégré, plugin, personnalisé)
- Les paramètres qu'il accepte
- Les signatures indiquant les types de données acceptés en entrée et en sortie
- Et plus encore

Vous pouvez afficher _toutes_ les commandes (sauf les externes) sous forme d'un seul grand tableau avec :

```nu
help commands
```

::: tip
Notez que les colonnes `params` et `input_output` de la sortie ci-dessus sont des tableaux _imbriqués_. Nushell permet des [structures de données imbriquées arbitrairement](./navigating_structured_data.md#background).
:::

## Explorer avec `explore`

La sortie de `help commands` est assez longue. Vous pourriez l'envoyer à un paginateur comme `less` ou `bat`, mais Nushell inclut une commande intégrée `explore` qui vous permet non seulement de faire défiler, mais aussi de plonger dans les données imbriquées. Essayez :

```nu
help commands | explore
```

Appuyez ensuite sur la touche <kbd>Entrée</kbd> pour accéder aux données elles-mêmes. Utilisez les touches fléchées pour faire défiler jusqu'à la commande `cp`, puis jusqu'à la colonne `params`. Appuyez à nouveau sur <kbd>Entrée</kbd> pour plonger dans la liste complète des paramètres disponibles pour la commande `cp`.

::: note
Appuyer une fois sur <kbd>Échap</kbd> revient du mode Défilement à la Vue ; appuyer une deuxième fois revient à la vue précédente (ou quitte, si on est déjà au niveau supérieur).
:::

::: tip
Vous pouvez bien sûr utiliser la commande `explore` sur _n'importe quelle_ donnée structurée dans Nushell : données JSON provenant d'une API Web, tableur ou fichier CSV, YAML, ou tout ce qui peut être représenté comme données structurées dans Nushell.

Essayez `$env.config | explore` pour le plaisir !
:::
