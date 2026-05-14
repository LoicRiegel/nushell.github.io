# Création de modules

[[toc]]

::: important
Lors des exemples ci-dessous, il est recommandé de démarrer un nouveau shell avant d'importer une version mise à jour de chaque module ou commande. Cela aidera à réduire toute confusion causée par les définitions des imports précédents.
:::

## Aperçu

Les modules (et les sous-modules, traités plus bas) sont créés de l'une des deux façons suivantes :

- Le plus souvent, en créant un fichier avec une série d'instructions `export` définissant les éléments à exporter depuis le module.
- Pour les sous-modules à l'intérieur d'un module, en utilisant la commande `module`

::: tip
Bien qu'il soit possible d'utiliser la commande `module` pour créer un module directement en ligne de commande, il est bien plus utile et courant de stocker les définitions du module dans un fichier pour la réutilisabilité.
:::

Le fichier de module peut être soit :

- Un fichier nommé `mod.nu`, auquel cas son _répertoire_ devient le nom du module
- Tout autre fichier `<nom_du_module>.nu`, auquel cas le nom du fichier devient le nom du module

### Exemple simple de module

Créez un fichier nommé `inc.nu` avec le contenu suivant :

```nu
export def increment []: int -> int  {
    $in + 1
}
```

C'est un module ! Nous pouvons maintenant l'importer et utiliser la commande `increment` :

```nu
use inc.nu *
5 | increment
# => 6
```

Bien sûr, vous pouvez facilement distribuer un tel fichier pour que d'autres puissent utiliser le module également.

## Exports

Nous avons brièvement couvert les types de définitions disponibles dans les modules dans l'aperçu principal des modules ci-dessus. Bien que cela puisse suffire pour un utilisateur final, les auteurs de modules devront savoir _comment_ créer les définitions d'export pour :

- Les commandes ([`export def`](/commands/docs/export_def.md))
- Les alias ([`export alias`](/commands/docs/export_alias.md))
- Les constantes ([`export const`](/commands/docs/export_const.md))
- Les externs connus ([`export extern`](/commands/docs/export_extern.md))
- Les sous-modules ([`export module`](/commands/docs/export_module.md))
- Les symboles importés d'autres modules ([`export use`](/commands/docs/export_use.md))
- La configuration de l'environnement ([`export-env`](/commands/docs/export-env.md))

::: tip
Seules les définitions marquées avec `export` (ou `export-env` pour les variables d'environnement) sont accessibles lorsque le module est importé. Les définitions non marquées avec `export` ne sont visibles qu'à l'intérieur du module. Dans certains langages, on les appellerait définitions « privées » ou « locales ». Un exemple se trouve ci-dessous dans [Exemples supplémentaires](#local-definitions).
:::

### Exports `main`

::: important
Un export ne peut pas avoir le même nom que le module lui-même.
:::

Dans l'[exemple de base](#simple-module-example) ci-dessus, nous avions un module nommé `inc` avec une commande nommée `increment`. Cependant, si nous renommons ce fichier en `increment.nu`, l'import échouera.

```nu
mv inc.nu increment.nu
use increment.nu *
# => Error: nu::parser::named_as_module
# => ...
# => help: Module increment can't export command named
# => the same as the module. Either change the module
# => name, or export `main` command.
```

Comme l'indique utilement le message d'erreur, il suffit de renommer l'export en `main`, auquel cas il prendra le nom du module lors de l'import. Éditez le fichier `increment.nu` :

```nu
export def main []: int -> int {
    $in + 1
}
```

Maintenant cela fonctionne comme prévu :

```nu
use ./increment.nu
2024 | increment
# => 2025
```

::: note
`main` peut être utilisé aussi bien pour les définitions `export def` que `export extern`.
:::

::: tip
Les définitions `main` sont importées dans les cas suivants :

- Le module entier est importé avec `use <module>` ou `use <module.nu>`
- Le glob `*` est utilisé pour importer toutes les définitions du module (par ex. `use <module> *`, etc.)
- La définition `main` est explicitement importée avec `use <module> main`, `use <module> [main]`, etc.)

À l'inverse, les formes suivantes n'importent _pas_ la définition `main` :

````nu
use <module> <other_definition>
# ou
use <module> [ <other_definitions> ]
:::

::: note
De plus, `main` a un comportement spécial dans un fichier script, qu'il soit exporté ou non. Voir le chapitre [Scripts](../scripts.html#parameterizing-scripts) pour plus de détails.
:::

## Fichiers de module

Comme mentionné brièvement dans l'aperçu ci-dessus, les modules peuvent être créés soit comme :

1. `<nom_du_module>.nu` : « Forme fichier » - Utile pour les modules simples
2. `<nom_du_module>/mod.nu` : « Forme répertoire » - Utile pour organiser des projets de modules plus grands où les sous-modules peuvent facilement correspondre aux sous-répertoires du module principal

L'exemple `increment.nu` ci-dessus est clairement un exemple de (1) la forme fichier. Essayons de le convertir en forme répertoire :

```nu
mkdir increment
mv increment.nu increment/mod.nu

use increment *
41 | increment
# => 42
````

Remarquez que le comportement du module une fois importé est identique que la forme fichier ou la forme répertoire soit utilisée ; seul son chemin change.

::: note
Techniquement, vous pouvez l'importer soit en utilisant la forme répertoire ci-dessus, soit explicitement avec `use increment/mod.nu *`, mais le raccourci de répertoire est préféré lors de l'utilisation d'un `mod.nu`.
:::

## Sous-commandes

Comme couvert dans [Commandes personnalisées](../custom_commands.md), les sous-commandes nous permettent de regrouper des commandes logiquement. En utilisant des modules, cela peut être fait de l'une des deux façons suivantes :

1. Comme pour toute commande personnalisée, la commande peut être définie comme `"<commande> <sous-commande>"`, en utilisant un espace à l'intérieur des guillemets. Ajoutons une sous-commande `increment by` au module `increment` défini ci-dessus :

```nu
export def main []: int -> int {
    $in + 1
}

export def "increment by" [amount: int]: int -> int {
    $in + $amount
}
```

Elle peut ensuite être importée avec `use increment *` pour charger à la fois la commande `increment` et la sous-commande `increment by`.

2. Alternativement, nous pouvons définir la sous-commande en utilisant simplement le nom `by`, car l'import du module `increment` entier résultera dans les mêmes commandes :

```nu
export def main []: int -> int {
    $in + 1
}

export def by [amount: int]: int -> int {
    $in + $amount
}
```

Ce module est importé avec `use increment` (sans le glob `*`) et résulte dans les mêmes commande `increment` et sous-commande `increment by`.

::: note
Nous continuerons à utiliser cette version pour les exemples suivants, donc notez que le pattern d'import a changé en `use increment` (plutôt que `use increment *`) ci-dessous.
:::

## Sous-modules

Les sous-modules sont des modules exportés depuis un autre module. Il y a deux façons d'ajouter un sous-module à un module :

1. Avec `export module` : Exporte (a) le sous-module et (b) ses définitions en tant que membres du sous-module
2. Avec `export use` : Exporte (a) le sous-module et (b) ses définitions en tant que membres du module parent

Pour démontrer la différence, créons un nouveau module `my-utils`, avec notre exemple `increment` comme sous-module. De plus, nous créerons une nouvelle commande `range-into-list` dans son propre sous-module.

1. Créez un répertoire pour le nouveau `my-utils` et déplacez `increment.nu` dedans

   ```nu
   mkdir my-utils
   # Ajustez ce qui suit selon les besoins
   mv increment/mod.nu my-utils/increment.nu
   rm increment
   cd my-utils
   ```

2. Dans le répertoire `my-utils`, créez un fichier `range-into-list.nu` avec le contenu suivant :

   ```nu
   export def main []: range -> list {
       # Cela semble étrange, oui, mais ce qui suit est simplement
       # une façon simple de convertir des plages en listes
       each {||}
   }
   ```

3. Testez-le :

   ```nu
   use range-into-list.nu
   1..5 | range-into-list | describe
   # => list<int> (stream)
   ```

4. Nous devrions maintenant avoir un répertoire `my-utils` avec :

   - Le module `increment.nu`
   - Le module `range-into-list.nu`

Les exemples suivants montrent comment créer un module avec des sous-modules.

### Exemple : Sous-module avec `export module`

La forme la plus courante pour une définition de sous-module est avec `export module`.

1. Créez un nouveau module nommé `my-utils`. Comme nous sommes dans le répertoire `my-utils`, nous allons créer un `mod.nu` pour le définir. Cette version de `my-utils/mod.nu` contiendra :

   ```nu
   export module ./increment.nu
   export module ./range-into-list.nu
   ```

2. Nous avons maintenant un module `my-utils` avec les deux sous-modules. Essayez-le :

   ```nu
   # Aller dans le répertoire parent de my-utils
   cd ..
   use my-utils *
   5 | increment by 4
   # => 9

   let file_indices = 0..2..<10 | range-into-list
   ls | select ...$file_indices
   # => Retourne les 1er, 3e, 5e, 7e et 9e fichiers du répertoire
   ```

Avant de passer à la section suivante, exécutez `scope modules` et recherchez le module `my-utils`. Remarquez qu'il n'a pas de commandes propres ; seulement les deux sous-modules.

### Exemple : Sous-module avec `export use`

Alternativement, nous pouvons (ré)exporter les _définitions_ d'autres modules. Cela diffère légèrement de la première forme, en ce que les commandes (et autres définitions, si présentes) de `increment` et `range-into-list` deviennent des _membres_ du module `my-utils` lui-même. Nous pourrons voir la différence dans la sortie de la commande `scope modules`.

Modifions `my-utils/mod.nu` en :

```nu
export use ./increment.nu
export use ./range-into-list.nu
```

Essayez-le en utilisant les mêmes commandes qu'avant :

```nu
# Aller dans le répertoire parent de my-utils
cd ..
use my-utils *
5 | increment by 4
# => 9

let file_indices = 0..2..<10 | range-into-list
ls / | sort-by modified | select ...$file_indices
# => Retourne les 1er, 3e, 5e, 7e et 9e fichiers du répertoire, du plus ancien au plus récent
```

Exécutez à nouveau `scope modules` et remarquez que toutes les commandes des sous-modules sont ré-exportées dans le module `my-utils`.

::: tip
Bien que `export module` soit la forme recommandée et la plus courante, il existe un scénario de conception de module dans lequel `export use` est requis -- `export use` peut être utilisé pour _exporter sélectivement_ des définitions depuis le sous-module, ce que `export module` ne peut pas faire. Voir [Exemples supplémentaires - Export sélectif](#selective-export-from-a-submodule) pour un exemple.
:::

::: note
`module` sans `export` ne définit qu'un module local ; il n'exporte pas de sous-module.
:::

## Documentation des modules

Comme pour les [commandes personnalisées](../custom_commands.md#documenting-your-command), les modules peuvent inclure une documentation consultable avec `help <nom_du_module>`. La documentation est simplement une série de lignes commentées au début du fichier de module. Documentons le module `my-utils` :

```nu
# Une collection de fonctions utilitaires utiles

export use ./increment.nu
export use ./range-into-list.nu
```

Examinez maintenant l'aide :

```nu
use my-utils *
help my-utils
# => A collection of helpful utility functions
```

Remarquez également que, parce que les commandes de `increment` et `range-into-list` sont ré-exportées avec `export use ...`, ces commandes apparaissent également dans l'aide du module principal.

## Variables d'environnement

Les modules peuvent définir un environnement avec [`export-env`](/commands/docs/export-env.md). Étendons notre module `my-utils` avec un export de variable d'environnement pour un répertoire commun où nous placerons nos modules à l'avenir. Ce répertoire est (par défaut) dans le chemin de recherche `$NU_LIB_DIRS` traité dans [Utilisation de modules - Chemin du module](./using_modules.md#module-path).

```nu
# Une collection de fonctions utilitaires utiles

export use ./increment.nu
export use ./range-into-list.nu

export-env {
    $env.NU_MODULES_DIR = ($nu.default-config-dir | path join "scripts")
}
```

Lorsque ce module est importé avec `use`, le code à l'intérieur du bloc [`export-env`](/commands/docs/export-env.md) est exécuté et son environnement est fusionné dans la portée courante :

```nu
use my-utils
$env.NU_MODULES_DIR
# => Retourne le nom du répertoire
cd $env.NU_MODULES_DIR
```

::: tip
Comme pour toute commande définie sans `--env`, les commandes et autres définitions du module utilisent leur propre portée pour l'environnement. Cela permet d'effectuer des modifications internes au module sans qu'elles se propagent dans la portée de l'utilisateur. Ajoutez ce qui suit au bas de `my-utils/mod.nu` :

```nu
export def examine-config-dir [] {
    # Change la variable d'environnement PWD
    cd $nu.default-config-dir
    ls
}
```

L'exécution de cette commande change le répertoire _localement_ dans le module, mais les modifications ne sont pas propagées à la portée parente.

:::

## Mises en garde

### `export-env` ne s'exécute que lorsque l'appel `use` est _évalué_

::: note
Ce scénario est fréquemment rencontré lors de la création d'un module qui utilise `std/log`.
:::

Tenter d'importer l'environnement d'un module dans un autre environnement peut ne pas fonctionner comme prévu. Créons un nouveau module `go.nu` qui crée des « raccourcis » vers des répertoires courants. L'un d'eux sera le `$env.NU_MODULES_DIR` défini ci-dessus dans `my-utils`.

Nous pourrions essayer :

```nu
# go.nu, dans le répertoire parent de my-utils
use my-utils

export def --env home [] {
    cd ~
}

export def --env modules [] {
    cd $env.NU_MODULES_DIR
}
```

Et ensuite l'importer :

```nu
use go.nu
go home
# => Fonctionne
go modules
# => Error: $env.NU_MODULES_DIR is not found
```

Cela ne fonctionne pas parce que `my-utils` n'est pas _évalué_ dans ce cas ; il est seulement _analysé_ lorsque le module `go.nu` est importé. Bien que cela amène tous les autres exports en portée, cela n'_exécute_ pas le bloc `export-env`.

::: important
Comme mentionné au début de ce chapitre, essayer cela alors que `my-utils` (et son `$env.NU_MODULES_DIR`) est encore en portée depuis un import précédent n'_échouera pas_ comme prévu. Testez dans une nouvelle session shell pour voir l'échec « normal ».
:::

Pour amener l'environnement exporté de `my-utils` en portée pour le module `go.nu`, il y a deux options :

1. Importer le module dans chaque commande où il est nécessaire

   En plaçant `use my-utils` dans la commande `go home` elle-même, son `export-env` sera _évalué_ lors de l'exécution de la commande. Par exemple :

   ```nu
   # go.nu
   export def --env home [] {
       cd ~
   }

   export def --env modules [] {
       use my-utils
       cd $env.NU_MODULES_DIR
   }
   ```

2. Importer l'environnement de `my-utils` à l'intérieur d'un bloc `export-env` dans le module `go.nu`

   ```nu
   use my-utils
   export-env {
       use my-utils []
   }

   export def --env home [] {
       cd ~
   }

   export def --env modules [] {
       cd $env.NU_MODULES_DIR
   }
   ```

   Dans l'exemple ci-dessus, `go.nu` importe `my-utils` deux fois :

   1. Le premier `use my-utils` importe le module et ses définitions (sauf l'environnement) dans la portée du module.
   2. Le second `use my-utils []` n'importe rien _sauf_ l'environnement dans le bloc d'environnement exporté de `go.nu`. Comme le `export-env` de `go.nu` est exécuté lors du premier import du module, le `use my-utils []` est également évalué.

Notez que la première méthode conserve l'environnement de `my-utils` à l'intérieur de la portée du module `go.nu`. La seconde, en revanche, ré-exporte l'environnement de `my-utils` dans la portée de l'utilisateur.

### Les fichiers et commandes de module ne peuvent pas avoir le même nom que le module parent

Un fichier `.nu` ne peut pas avoir le même nom que son répertoire de module (par ex. `spam/spam.nu`) car cela créerait une condition ambiguë avec le nom défini deux fois. Cela est similaire à la situation décrite ci-dessus où une commande ne peut pas avoir le même nom que son parent.

## Syntaxe de chemin Windows

::: important
Nushell sur Windows supporte les barres obliques avant et arrière comme séparateur de chemin. Cependant, pour s'assurer qu'ils fonctionnent sur toutes les plateformes, l'utilisation uniquement de la barre oblique avant `/` dans vos modules est fortement recommandée.
:::

## Exemples supplémentaires

### Définitions locales

Comme mentionné ci-dessus, les définitions dans un module sans le mot-clé [`export`](/commands/docs/export.md) ne sont accessibles que dans la portée du module.

Pour le démontrer, créez un nouveau module `is-alphanumeric.nu`. À l'intérieur de ce module, nous allons créer une commande `str is-alphanumeric`. Si l'un des caractères de la chaîne n'est pas alphanumérique, elle retourne `false` :

```nu
# is-alphanumeric.nu
def alpha-num-range [] {
    [
        ...(seq char 'a' 'z')
        ...(seq char 'A' 'Z')
        ...(seq 0 9 | each { into string })
    ]
}

export def "str is-alphanumeric" []: string -> bool {
    if ($in == '') {
        false
    } else {
        let chars = (split chars)
        $chars | all {|char| $char in (alpha-num-range)}
    }
}
```

Remarquez que nous avons deux définitions dans ce module -- `alpha-num-range` et `str is-alphanumeric`, mais seule la seconde est exportée.

```nu
use is-alphanumeric.nu *
'Word' | str is-alphanumeric
# => true
'Some punctuation?!' | str is-alphanumeric
# => false
'a' in (alpha-num-range)
# => Error:
# => help: `alpha-num-range` is neither a Nushell built-in or a known external command
```

### Export sélectif depuis un sous-module

::: note
Bien que ce qui suit soit un cas d'usage rare, cette technique est utilisée par la bibliothèque standard pour
rendre les commandes `dirs` et ses alias disponibles séparément.
:::

Comme mentionné dans la section [Sous-modules](#submodules) ci-dessus, seul `export use` peut exporter sélectivement des définitions depuis un sous-module.

Pour le démontrer, ajoutons une forme modifiée de l'exemple de module `go.nu` [ci-dessus](#caveats) à `my-utils` :

```nu
# go.nu, dans le répertoire my-utils
export def --env home [] {
    cd ~
}

export def --env modules [] {
    cd ($nu.default-config-dir | path join "scripts")
}

export alias h = home
export alias m = modules
```

Ce `go.nu` inclut les modifications suivantes par rapport à l'original :

- Il ne repose pas sur le module `my-utils` car il sera maintenant un sous-module de `my-utils` à la place
- Il ajoute des alias « raccourcis » :
  `h` : Va dans le répertoire personnel (alias de `go home`)
  `m` : Va dans le répertoire des modules (alias de `go modules`)

Un utilisateur pourrait importer _uniquement_ les alias avec :

```nu
use my-utils/go.nu [h, m]
```

Cependant, disons que nous voulons que `go.nu` soit un sous-module de `my-utils`. Lorsqu'un utilisateur importe `my-utils`, il devrait _uniquement_ obtenir les commandes, mais pas les alias. Éditez `my-utils/mod.nu` et ajoutez :

```nu
export use ./go.nu [home, modules]
```

Cela _presque_ fonctionne -- Cela exporte sélectivement `home` et `modules`, mais pas les alias. Cependant, cela le fait sans le préfixe `go`. Par exemple :

```nu
use my-utils *
home
# => fonctionne
go home
# => Error: command not found
```

Pour les exporter en tant que `go home` et `go modules`, faites la modification suivante dans `my-utils/mod.nu` :

```nu
# Remplacer l'`export use` existant par ...
export module go {
    export use ./go.nu [home, modules]
}
```

Cela crée un nouveau sous-module exporté `go` dans `my-utils` avec les définitions (ré)exportées sélectivement pour `go home` et `go modules`.

```nu
use my-utils *
# => Comme prévu :
go home
# => fonctionne
home
# => Error: command not found
```
