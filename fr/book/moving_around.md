# Naviguer dans le Système

Une caractéristique intrinsèque d'un shell est la capacité à naviguer et interagir avec le système de fichiers. Nushell ne fait évidemment pas exception. Voici quelques commandes courantes que vous pourriez utiliser pour interagir avec le système de fichiers :

## Affichage du Contenu d'un Répertoire

```nu
ls
```

Comme vu dans le Rapide Tour d'Horizon, la commande [`ls`](/commands/docs/ls.md) retourne le contenu d'un répertoire. La commande `ls` de Nushell retourne les éléments sous forme de [tableau](/book/types_of_data.html#tables).

La commande [`ls`](/commands/docs/ls.md) accepte également un argument optionnel pour spécifier ce que vous souhaitez afficher. Par exemple, nous pouvons lister les fichiers se terminant par « .md » :

```nu
ls *.md
# => ╭───┬────────────────────┬──────┬──────────┬──────────────╮
# => │ # │        name        │ type │   size   │   modified   │
# => ├───┼────────────────────┼──────┼──────────┼──────────────┤
# => │ 0 │ CODE_OF_CONDUCT.md │ file │  3.4 KiB │ 9 months ago │
# => │ 1 │ CONTRIBUTING.md    │ file │ 11.0 KiB │ 5 months ago │
# => │ 2 │ README.md          │ file │ 12.0 KiB │ 6 days ago   │
# => │ 3 │ SECURITY.md        │ file │  2.6 KiB │ 2 months ago │
# => ╰───┴────────────────────┴──────┴──────────┴──────────────╯
```

## Patterns de Glob (wildcards)

L'astérisque (`*`) dans l'argument optionnel `*.md` est parfois appelé caractère générique, wildcard, ou glob. Il nous permet de matcher n'importe quoi. Vous pouvez lire ce glob `*.md` comme _"match n'importe quel nom de fichier, du moment qu'il se termine par '.md'."_

Le glob le plus général est `*`, qui matchera tous les chemins. Vous verrez souvent ce pattern utilisé comme partie d'un autre pattern, par exemple `*.bak` et `temp*`.

Nushell prend également en charge une double `*`, qui traversera les chemins imbriqués dans d'autres répertoires. Par exemple, `ls **/*` listera tous les chemins non cachés sous le répertoire courant.

```nu
 ls **/*.md
╭───┬───────────────────────────────┬──────┬──────────┬──────────────╮
│ # │             name              │ type │   size   │   modified   │
├───┼───────────────────────────────┼──────┼──────────┼──────────────┤
│ 0 │ CODE_OF_CONDUCT.md            │ file │  3.4 KiB │ 5 months ago │
│ 1 │ CONTRIBUTING.md               │ file │ 11.0 KiB │ a month ago  │
│ 2 │ README.md                     │ file │ 12.0 KiB │ a month ago  │
│ 3 │ SECURITY.md                   │ file │  2.6 KiB │ 5 hours ago  │
│ 4 │ benches/README.md             │ file │    249 B │ 2 months ago │
│ 5 │ crates/README.md              │ file │    795 B │ 5 months ago │
│ 6 │ crates/nu-cli/README.md       │ file │    388 B │ 5 hours ago  │
│ 7 │ crates/nu-cmd-base/README.md  │ file │    262 B │ 5 hours ago  │
│ 8 │ crates/nu-cmd-extra/README.md │ file │    669 B │ 2 months ago │
│ 9 │ crates/nu-cmd-lang/README.md  │ file │  1.5 KiB │ a month ago  │
╰───┴───────────────────────────────┴──────┴──────────┴──────────────╯
```

Ici, nous cherchons tout fichier se terminant par ".md". Les doubles astérisques spécifient en outre _"dans n'importe quel répertoire à partir d'ici"_.

La syntaxe de globbing de Nushell prend non seulement en charge `*`, mais aussi les matching de [caractères uniques avec `?` et les groupes de caractères avec `[...]`](https://docs.rs/nu-glob/latest/nu_glob/struct.Pattern.html).

Pour échapper les patterns `*`, `?`, et `[]`, on les entoure de guillemets simples, doubles ou [string raw](working_with_strings.md#raw-strings). Par exemple, pour afficher le contenu d'un répertoire nommé `[slug]`, utilisez `ls "[slug]"` ou `ls '[slug]'`.

Cependant, les chaînes entre _backtick_ ne permettent pas d'échapper les globs. Comparez les scénarios suivants :

1. Sans guillemets : pattern de Glob

   Une [chaîne contenant des simples mots](/book/working_with_strings.html#bare-word-strings) sans guillemets contenent des caractères glob est interprétée comme un pattern de glob. Par exemple, la commande suivante supprimera tous les fichiers dans le répertoire courant contenant `myfile` dans leur nom :

   ```nu
   rm *myfile*
   ```

2. Avec guillemets : chaine literal avec astérisques

   Avec des guillemets simples et ou doubles, ou en utilisant des [strings raw](/book/working_with_strings.html#raw-strings), une _string_ littérale, avec les astérisques (ou tout autre catactère de glob) échappés, est passée à la commande. Le résultat n'est pas un glob. La commande suivante va seulement supprimer un fichier littéralement nommé `*myfile*` (astérisques inclus). D'autres fichiers avec `myfile` dans leur nom ne seront pas affectés :

   ```nu
   rm "*myfile*"
   ```

3. Entre backticks : pattern de Glob

   Les astérisques (et autre patterns de glob) à l'intérieur d'une [string entre backticks](/book/working_with_strings.html#backtick-quoted-strings) sont interprétés comme pattern de glob. Notez que le comportement est le même que dans l'exemple d'une chaîne sans guillemets, #1 ci-dessus.

   La suivante, comme dans le premier exemple, supprime tous les fichiers contenant `myfile` dans leur nom dans le répertoire actuel .

   ```nu
   rm `*myfile*`
   ```

::: tip
Nushell inclue également une [commande `glob`](https://www.nushell.sh/commands/docs/glob.html) dédiée, qui prend en charge des scénarios de globbing plus complexes.
:::

### Conversion de Chaînes en Globs

Les techniques utilisant des guillemets ci-dessus sont utiles pour construire des globs littéraux, mais vous pourriez avoir besoin de construire des globs de mamière programmable.
Voici quelques techniques pour faire cela :

1. `into glob`

   La [commande `into glob`](/commands/docs/into_glob.html) peut être utilisée pour convertir pour convertir une chaine (ou d'autres types) en glob. Par exemple :

   ```nu
   # Find files whose name includes the current month in the form YYYY-mm
   let current_month = (date now | format date '%Y-%m')
   let glob_pattern = ($"*($current_month)*" | into glob)
   ls $glob_pattern
   ```

2. L'opérateur de propagation combiné avec la [commande `glob`](/commands/docs/glob.html) :

   La [commande `glob`](/commands/docs/glob.html) (note : différente de `into glob`) produit une [`liste`](/book/types_of_data.html#lists) de noms de fichiers qui matchent le pattern de glob. Cette liste peut être étendue et passée aux commandes du système de fichiers en utilisant [l'opérateur de propagation](/book/operators.html#spread-operator) :

   ```nu
   # Find files whose name includes the current month in the form YYYY-mm
   let current_month = (date now | format date '%Y-%m')
   ls ...(glob $"*($current_month)*")
   ```

3. Forcer le type de `glob` via des annotations :

   ```nu
   # Find files whose name includes the current month in the form YYYY-mm
   let current_month = (date now | format date '%Y-%m')
   let glob_pattern: glob = ($"*($current_month)*")
   ls $glob_pattern
   ```

## Créer un Répertoire

Comme dans la plupart des autres shells, la commande [`mkdir`](/commands/docs/mkdir.md) est utilisée pour créer de nouveaux répertoires. Une différence subtile est que la commande `mkdir` interne de Nushell fonctionne comme le `mkdir -p` Unix/Linux par défaut, c'est-à-dire qu'elle :

- Crée automatiquement plusieurs niveaux de répertoires. Par exemple :

  ```nu
  mkdir modules/my/new_module
  ```

  Cela créera les trois répertoires même si aucun n'existe encore. Sous Linux/Unix, cela nécessite `mkdir -p`.

- Ne génère pas d'erreur si le répertoire existe déjà. Par exemple :

  ```nu
  mkdir modules/my/new_module
  mkdir modules/my/new_module
  # => Pas d'erreur
  ```

  ::: tip
  Une erreur courante lors de l'arrivée sur Nushell est d'essayer d'utiliser `mkdir -p <répertoire>` comme dans la version native Linux/Unix. Cela génèrera une erreur `Unknown Flag` dans Nushell.

  Répétez simplement la commande sans `-p` pour obtenir le même effet.
  :::

## Changer le Répertoire Courant

```nu
cd cookbook
```

Pour changer du répertoire courant vers un nouveau, utilisez la commande [`cd`](/commands/docs/cd.md).

Changer le répertoire courant peut également être effectué si [`cd`](/commands/docs/cd.md) est omis et seul un chemin est fourni :

```nu
cookbook/
```

Exactement comme dans d'autres shells, vous pouvez utiliser le nom d'un répertoire, ou si vous voulez remonter d'un répertoire, vous pouvez utiliser le raccourci `..`.

Vous pouvez ajouter des points supplémentaires pour remonter de niveaux supplémentaires :

```nu
# Remonter au répertoire parent
cd ..
# ou
..
# Remonter de deux niveaux (parent du parent)
cd ...
# ou
...
# Remonter de trois niveaux (parent du parent du parent)
cd ....
# Etc.
```

::: tip
Les raccourcis multi-points sont disponibles à la fois pour les commandes Nushelles, [les commandes du système de fichier](/commands/categories/filesystem.html) et les commandes externes. Par exemple, executer `^stat ....` sur un système Linux/Unix affichera que le chemin est étendu à `../../../..`
:::

Vous pouvez aussi combiner des niveaux de répertoires relatifs avec des noms de répertoires :

```nu
cd ../sibling
```

::: tip CONSEIL IMPORTANT
Changer le répertoire courant avec [`cd`](/commands/docs/cd.md) change aussi la variable d'environnement `PWD`. Cela signifie que le changement de répertoire est restreint au scope actuel (par ex. bloc ou closure). Une fois que vous êtes sortis de ce bloc, vous allez retourner au répertoire précédent. Vous pouvez en apprendre plus au chapitre [Environnement](/book/environment.md).
:::

## Commandes de Système de Fichiers

Nu met également à disposition des commandes basiques de [système de fichiers](/commands/categories/filesystem.html) qui fonctionnent sur toutes les plateformes, telles que :

- [`mv`](/commands/docs/mv.md) pour renommer ou déplacer un fichier ou répertoire vers un nouvel emplacement
- [`cp`](/commands/docs/cp.md) pour copier un élément vers un nouvel emplacement
- [`rm`](/commands/docs/rm.md) pour supprimer des éléments du système de fichiers

::: tip NOTE
Sous Bash et de nombreux autres shells, la plupart des commandes de système de fichiers (excepté `cd`) sont en fait des binaires séparés dans le système. Par exemple, sur un système Linux, `cp` est le binaire `/usr/bin/cp`. Dans Nushell, ces commandes sont intégrées. Cela a plusieurs avantages :

- Elles fonctionnent de manière cohérente sur les plateformes où une version binaire n'est peut-être pas disponible (par ex. Windows). Cela permet la création de scripts, modules ou commandes personnalisées multiplateformes.
- Elles sont intégrées plus étroitement avec Nushell, leur permettant de comprendre les types et constructions de Nushell.
- Comme mentionné dans le [Rapide Tour d'Horizon](quick_tour.html), elles sont documentées dans le système d'aide de Nushell. Exécuter `help <command>` ou `<command> --help` affichera la documentation Nushell pour ces commandes.

Bien que l'utilisation des versions intégrées à Nushell soit généralement recommandée, il est possible d'accéder aux binaires Linux. Lisez [Exécuter des Commandes Système](./running_externals.md) pour les détails.
