# Utilisation des modules

[[toc]]

## Aperçu

Les utilisateurs finaux peuvent ajouter de nouvelles fonctionnalités à Nushell en utilisant (« important ») des modules écrits par d'autres.

Pour importer un module et ses définitions, nous appelons la commande [`use`](/commands/docs/use.md) :

```nu
use <chemin/vers/module> <membres...>
```

Par exemple :

```nu
use std/log
log info "Hello, Modules"
```

::: tip
L'exemple ci-dessus utilise la [Bibliothèque standard](../standard_library.md), une collection de modules intégrés à Nushell. Comme elle est facilement accessible à tous les utilisateurs de Nushell, nous l'utiliserons également pour plusieurs des exemples ci-dessous.
:::

## Installation de modules

Installer un module consiste simplement à placer ses fichiers dans un répertoire. Cela peut être fait via `git clone` (ou un autre système de contrôle de version), un gestionnaire de paquets comme `nupm`, ou manuellement. La documentation du module devrait fournir des recommandations.

## Importation de modules

Tout ce qui suit le mot-clé [`use`](/commands/docs/use.md) forme un **pattern d'import** qui contrôle comment les définitions sont importées.

Remarquez ci-dessus que `use` a deux arguments :

- Un chemin vers le module
- (Optionnel) Les définitions à importer

La documentation du module vous indiquera généralement la façon recommandée de l'importer. Cependant, il peut être utile de comprendre les options disponibles :

### Chemin du module

Le chemin vers le module peut être :

- Un chemin absolu vers un répertoire contenant un fichier `mod.nu` :

  ::: details Exemple

  ```nu
  use ~/nushell/modules/nupm
  ```

  Notez que le nom du module (c'est-à-dire son répertoire) peut se terminer par `/` (ou `\` sur Windows), mais comme pour la plupart des commandes prenant des chemins (par ex. `cd`), c'est complètement optionnel.

  :::

- Un chemin relatif vers un répertoire contenant un fichier `mod.nu` :

  ::: details Exemple

  ```nu
  # cd puis utiliser le mod.nu dans le répertoire nupm relatif
  cd ~/nushell/modules
  use nupm
  # ou
  use nupm/
  ```

  Notez que le nom du module (son répertoire) peut se terminer par `/` (ou `\` sur Windows), mais comme pour la plupart des commandes prenant un chemin (par ex. `cd`), c'est complètement optionnel.
  :::

  ::: important Important ! Importation de modules depuis `$NU_LIB_DIRS` ou `$env.NU_LIB_DIRS`
  Lors de l'importation d'un module via un chemin relatif, Nushell cherche d'abord depuis le répertoire courant. Si un module correspondant n'est pas trouvé à cet emplacement, Nushell cherche ensuite dans chaque répertoire de la liste constante `$NU_LIB_DIRS`, puis dans `$env.NU_LIB_DIRS` (déprécié).

  Cela vous permet d'installer des modules dans un emplacement facilement accessible via un chemin relatif quel que soit le répertoire courant.
  :::

- Un chemin absolu ou relatif vers un fichier de module Nushell. Comme ci-dessus, Nushell cherchera dans la constante `$NU_LIB_DIRS` puis dans `$env.NU_LIB_DIRS` pour un chemin relatif correspondant.

  ::: details Exemple

  ```nu
  use ~/nushell/modules/std-rfc/bulk-rename.nu
  # Ou
  cd ~/nushell/modules
  use std-rfc/bulk-rename.nu
  ```

  :::

- Un répertoire virtuel :

  ::: details Exemple
  Les modules de la bibliothèque standard mentionnés ci-dessus sont stockés dans un système de fichiers virtuel avec un répertoire `std`. Considérez ceci comme une forme alternative des exemples de « chemin absolu » ci-dessus.

  ```nu
  use std/assert
  assert equal 'string1' "string1"
  ```

  :::

- Moins fréquemment, le nom d'un module déjà créé avec la commande [`module`](/commands/docs/module.md). Bien qu'il soit possible d'utiliser cette commande pour créer un module en ligne de commande, ce n'est pas courant ni utile. À la place, cette forme est principalement utilisée par les auteurs de modules pour définir un sous-module. Voir [Création de modules - Sous-modules](./creating_modules.md#submodules).

### Définitions du module

Le second argument de la commande `use` est une liste optionnelle des définitions à importer. Encore une fois, la documentation du module devrait fournir des recommandations. Par exemple, le [chapitre sur la bibliothèque standard](../standard_library.md#importing-submodules) couvre les imports recommandés pour chaque sous-module.

Bien sûr, vous avez toujours la possibilité de choisir la forme qui convient le mieux à votre cas d'usage.

- **Importer un module/sous-module entier en tant que commande avec sous-commandes**

  Dans un exemple précédent ci-dessus, nous avons importé le module `std/log` sans spécifier les définitions :

  ```nu
  use std/log
  log info "Hello, std/log Module"
  ```

  Remarquez que cela importe le sous-module `log` avec toutes ses _sous-commandes_ (par ex. `log info`, `log error`, etc.) dans la portée courante.

  Comparez ce qui précède à la version suivante, où la commande devient `std log info` :

  ```nu
  use std
  std log info "Hello, std Module"
  ```

- **Importer toutes les définitions d'un module**

  Alternativement, vous pouvez importer les définitions elles-mêmes dans la portée courante. Par exemple :

  ```nu
  use std/formats *
  ls | to jsonl
  ```

  Remarquez comment la commande `to jsonl` est placée directement dans la portée courante, plutôt qu'être une sous-commande de `formats`.

- **Importer une ou plusieurs définitions d'un module**

  Nushell peut également importer sélectivement un sous-ensemble des définitions d'un module. Par exemple :

  ```nu
  use std/math PI
  let circle = 2 * $PI * $radius
  ```

  Gardez à l'esprit que les définitions peuvent être :

  - Des commandes
  - Des alias
  - Des constantes
  - Des externs
  - D'autres modules (en tant que sous-modules)
  - Des variables d'environnement (toujours importées)

  Moins fréquemment, une liste d'imports peut également être utilisée :

  ```nu
  use std/formats [ 'from ndjson' 'to ndjson' ]
  ```

  ::: note Importation de sous-modules
  Bien que vous puissiez importer un sous-module seul en utilisant `use <module> <sous-module>` (par ex. `use std help`), le module parent entier et _toutes_ ses définitions (et donc sous-modules) seront _analysés_ lors de l'utilisation de cette forme. Dans la mesure du possible, charger le sous-module comme un _module_ résultera en un code plus rapide. Par exemple :

  ```nu
  # Plus rapide
  use std/help
  ```

  :::

## Importation de constantes

Comme vu ci-dessus avec les exemples `std/math`, certains modules peuvent exporter des définitions constantes. Lors de l'importation du module entier, les constantes sont accessibles via un record du même nom que le module :

```nu
# Importation du module entier - Accès par record
use std/math
$math.PI
# => 3.141592653589793

$math
# => ╭───────┬──────╮
# => │ GAMMA │ 0.58 │
# => │ E     │ 2.72 │
# => │ PI    │ 3.14 │
# => │ TAU   │ 6.28 │
# => │ PHI   │ 1.62 │
# => ╰───────┴──────╯

# Ou importation de tous les membres du module
use std/math *
$PI
# => 3.141592653589793
```

## Masquage

Toute commande personnalisée ou alias, qu'il soit importé depuis un module ou non, peut être « masqué » pour restaurer la définition précédente en utilisant la commande [`hide`](/commands/docs/hide.md).

La commande `hide` accepte également des patterns d'import, similaires à [`use`](/commands/docs/use.md), mais les interprète légèrement différemment. Ces patterns peuvent être l'un des suivants :

- Si le nom est une commande personnalisée, la commande `hide` la masque directement.
- Si le nom est un nom de module, elle masque tous ses exports préfixés avec le nom du module

Par exemple, en utilisant `std/assert` :

```nu
use std/assert
assert equal 1 2
# => Assertion failed
assert true
# => Assertion passes

hide assert
assert equal 1 1
# => Error:
# => help: A command with that name exists in module `assert`. Try importing it with `use`

assert true
# => Error:
# => help: A command with that name exists in module `assert`. Try importing it with `use`
```

Tout comme vous pouvez utiliser `use` pour un sous-ensemble des définitions du module, vous pouvez également les masquer sélectivement :

```nu
use std/assert
hide assert main
assert equal 1 1
# => assertion passes

assert true
# => Error:
# => help: A command with that name exists in module `assert`. Try importing it with `use`
```

::: tip
`main` est couvert plus en détail dans [Création de modules](./creating_modules.md#main-exports), mais pour les utilisateurs finaux, `main` signifie simplement « la commande portant le même nom que le module ». Dans ce cas, le module `assert` exporte une commande `main` qui « se fait passer pour » la commande `assert`. Masquer `main` a pour effet de masquer la commande `assert`, mais pas ses sous-commandes.
:::

## Voir aussi

- Pour qu'un module soit toujours disponible sans avoir à utiliser `use` à chaque session Nushell, ajoutez simplement son import (`use`) à votre configuration de démarrage. Voir le chapitre [Configuration](../configuration.md) pour savoir comment faire.

- Les modules peuvent également être utilisés comme partie d'un [Overlay](../overlays.md).
