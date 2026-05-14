# Penser en Nu

Nushell est différent ! Il est courant (et tout à fait normal !) que les nouveaux utilisateurs aient certaines habitudes ou modèles mentaux hérités d'autres shells ou langages.

Les questions les plus fréquentes des nouveaux utilisateurs tombent généralement dans l'une des catégories suivantes :

[[toc]]

## Nushell n'est pas Bash

### Il peut parfois ressembler à Bash

Nushell est à la fois un langage de programmation et un shell. Pour cette raison, il possède sa propre façon de travailler avec les fichiers, les répertoires, les sites web, et plus encore. Certaines fonctionnalités de Nushell ressemblent à celles que vous connaissez dans d'autres shells. Par exemple, les pipelines fonctionnent en enchaînant deux commandes (ou plus), comme dans les autres shells.

Par exemple, la ligne de commande suivante fonctionne de la même façon dans Bash et Nushell sur les plateformes Unix/Linux :

```nu
curl -s https://api.github.com/repos/nushell/nushell/contributors | jq -c '.[] | {login,contributions}'
# => returns contributors to Nushell, ordered by number of contributions
```

Nushell partage de nombreuses autres similitudes avec Bash (et d'autres shells) et dispose de nombreuses commandes communes.

::: tip
Bash est principalement un interpréteur de commandes qui exécute des commandes externes. Nushell en propose beaucoup sous forme de commandes intégrées multiplateformes.

Bien que la ligne de commande ci-dessus fonctionne dans les deux shells, dans Nushell il n'est pas nécessaire d'utiliser `curl` et `jq`. Nushell dispose en effet d'une commande intégrée [`http get`](/commands/docs/http_get.md) et gère nativement les données JSON. Par exemple :

```nu
http get https://api.github.com/repos/nushell/nushell/contributors | select login contributions
```

:::

::: warning Penser en Nushell
Nushell emprunte des concepts à de nombreux shells et langages. Vous trouverez probablement beaucoup de ses fonctionnalités familières.
:::

### Mais ce n'est pas Bash

Cependant, il est parfois facile d'oublier que certaines constructions de style Bash (et POSIX en général) ne fonctionnent tout simplement pas dans Nushell. Par exemple, en Bash, il serait normal d'écrire :

```sh
# Redirection avec >
echo "hello" > output.txt
# Mais comparaison (supérieur à) avec la commande test
test 4 -gt 7
echo $?
# => 1
```

Dans Nushell, en revanche, `>` est utilisé comme l'opérateur de comparaison « supérieur à ». C'est plus en accord avec les attentes des langages de programmation modernes.

```nu
4 > 10
# => false
```

Puisque `>` est un opérateur, la redirection vers un fichier dans Nushell est gérée via une commande de pipeline dédiée à la sauvegarde du contenu — [`save`](/commands/docs/save.md) :

```nu
"hello" | save output.txt
```

::: warning Penser en Nushell
Nous avons rassemblé une liste des constructions Bash courantes et leur équivalent Nushell dans le chapitre [Venir de Bash](./coming_from_bash.md).
:::

## Retour Implicite

Les utilisateurs venant d'autres shells seront sans doute très familiers avec la commande `echo`. La commande [`echo`](/commands/docs/echo.md) de Nushell peut sembler identique au premier abord, mais elle est _très_ différente.

Notez d'abord que la sortie suivante est _identique en apparence_ dans Bash, Nushell (et même PowerShell et Fish) :

```nu
echo "Hello, World"
# => Hello, World
```

Mais alors que les autres shells envoient `Hello, World` directement vers la _sortie standard_, `echo` de Nushell _retourne simplement une valeur_. Nushell _affiche_ ensuite la valeur de retour d'une commande, ou plus précisément, d'une _expression_.

Plus important encore, Nushell _retourne implicitement_ la valeur d'une expression. C'est similaire à PowerShell ou Rust à bien des égards.

::: tip
Une expression peut être bien plus qu'un simple pipeline. Même les commandes personnalisées (similaires aux fonctions dans d'autres langages, nous les couvrirons plus en détail dans un [chapitre ultérieur](./custom_commands.md)) retournent automatiquement et implicitement la dernière valeur. Pas besoin d'un `echo` ni d'une [commande `return`](/commands/docs/return.md) — cela se produit _naturellement_.
:::

En d'autres termes, la string _"Hello, World"_ et la valeur de sortie de `echo "Hello, World"` sont équivalentes :

```nu
"Hello, World" == (echo "Hello, World")
# => true
```

Voici un autre exemple avec une définition de commande personnalisée :

```nu
def latest-file [] {
    ls | sort-by modified | last
}
```

La _sortie_ de ce pipeline (sa _« valeur »_) devient la _valeur de retour_ de la commande personnalisée `latest-file`.

::: warning Penser en Nushell
La plupart du temps où vous écririez `echo <quelque chose>`, dans Nushell, vous pouvez simplement écrire `<quelque chose>` à la place.
:::

## Une Seule Valeur de Retour par Expression

Il est important de comprendre qu'une expression ne peut retourner qu'une seule valeur. Si une expression contient plusieurs sous-expressions, seule la **_dernière_** valeur est retournée.

Une erreur courante est d'écrire une commande personnalisée comme ceci :

```nu:line-numbers
def latest-file [] {
    echo "Returning the last file"
    ls | sort-by modified | last
}

latest-file
```

Les nouveaux utilisateurs pourraient s'attendre à ce que :

- La ligne 2 affiche _"Returning the last file"_
- La ligne 3 retourne/affiche le fichier

Cependant, souvenez-vous que `echo` **_retourne une valeur_**. Puisque seule la dernière valeur est retournée, la valeur de la ligne 2 est ignorée. Seul le fichier sera retourné par la ligne 3.

Pour s'assurer que la première ligne est bien _affichée_, utilisez la [commande `print`](/commands/docs/print.md) :

```nu
def latest-file [] {
    print "Returning last file"
    ls | sort-by modified | last
}
```

Comparez également :

```nu
40; 50; 60
```

::: tip
Un point-virgule est équivalent à un saut de ligne dans une expression Nushell. Ce qui précède est identique à :

```nu
40
50
60
```

ou

```nu
echo 40
echo 50
echo 60
```

Voir aussi : [Édition Multi-lignes](./line_editor.md#multi-line-editing)
:::

Dans tous les exemples ci-dessus :

- La première valeur est évaluée comme l'entier 40 mais n'est pas retournée
- La deuxième valeur est évaluée comme l'entier 50 mais n'est pas retournée
- La troisième valeur est évaluée comme l'entier 60, et comme c'est la dernière valeur, elle est retournée et affichée (rendue).

::: warning Penser en Nushell
Lors du débogage de résultats inattendus, recherchez :

- Des sous-expressions (commandes ou pipelines) qui...
- ...retournent une valeur (non `null`)...
- ...où cette valeur n'est pas retournée depuis l'expression parente.

Ce sont souvent des sources de problèmes dans votre code.
:::

## Chaque Commande Retourne une Valeur

Certains langages ont le concept d'« instructions » qui ne retournent pas de valeur. Nushell n'en a pas.

Dans Nushell, **_chaque commande retourne une valeur_**, même si cette valeur est `null` (le type `nothing`). Considérez l'expression multiligne suivante :

```nu:line-numbers
let p = 7
print $p
$p * 6
```

1. Ligne 1 : L'entier 7 est assigné à `$p`, mais la valeur de retour de la [commande `let`](/commands/docs/let.md) elle-même est `null`. Comme ce n'est pas la dernière valeur de l'expression, elle n'est pas affichée.
2. Ligne 2 : La valeur de retour de la commande `print` elle-même est `null`, mais `print` force l'affichage de son argument (`$p`, qui vaut 7). Comme à la ligne 1, la valeur `null` est ignorée car ce n'est pas la dernière valeur de l'expression.
3. Ligne 3 : S'évalue en la valeur entière 42. Comme dernière valeur de l'expression, c'est le résultat retourné, qui est également affiché (rendu).

::: warning Penser en Nushell
Se familiariser avec les types de sortie des commandes courantes vous aidera à comprendre comment combiner des commandes simples pour obtenir des résultats complexes.

`help <commande>` affiche la signature, y compris le(s) type(s) de sortie, pour chaque commande de Nushell.
:::

## Pensez à Nushell comme un Langage Compilé

Dans Nushell, il y a exactement deux étapes de haut niveau distinctes lors de l'exécution du code :

1. _Étape 1 (Parser) :_ Parser l'**_intégralité_** du code source
2. _Étape 2 (Moteur) :_ Évaluer l'**_intégralité_** du code source

Il peut être utile de considérer l'étape de parsing de Nushell comme la _compilation_ dans les langages [statiques](./how_nushell_code_gets_run.md#dynamic-vs-static-languages) comme Rust ou C++. En clair, tout le code qui sera évalué à l'étape 2 doit être **_connu et disponible_** pendant l'étape de parsing.

::: important
Cela signifie que Nushell ne peut pas prendre en charge une construction `eval` comme dans les langages _dynamiques_ tels que Bash ou Python.
:::

### Fonctionnalités fondées sur le Parsing Statique

En contrepartie, les résultats **_statiques_** du parsing sont la clé de nombreuses fonctionnalités de Nushell et de son REPL, notamment :

- Messages d'erreur précis et expressifs
- Analyse sémantique pour une détection précoce et robuste des erreurs
- Intégration IDE
- Le système de types
- Le système de modules
- Les complétions
- Le parsing des arguments de commandes personnalisées
- La coloration syntaxique
- La mise en évidence des erreurs en temps réel
- Les commandes de profilage et de débogage
- (Futur) Formatage
- (Futur) Sauvegarde de résultats IR (Représentation Intermédiaire) « compilés » pour une exécution plus rapide

### Limitations

La nature statique de Nushell entraîne souvent de la confusion chez les utilisateurs venant de langages où `eval` est disponible.

Considérons un fichier simple de deux lignes :

```text
<code ligne1>
<code ligne2>
```

1. Parsing :
   1. La ligne 1 est parsée
   2. La ligne 2 est parsée
2. Si le parsing a réussi, Évaluation :
   1. La ligne 1 est évaluée
   2. La ligne 2 est évaluée

Cela illustre pourquoi les exemples suivants ne peuvent pas s'exécuter comme une seule expression (par exemple, un script) dans Nushell :

::: note
Les exemples suivants utilisent la [commande `source`](/commands/docs/source.md), mais des conclusions similaires s'appliquent aux autres commandes qui parsent du code source Nushell, comme [`use`](/commands/docs/use.md), [`overlay use`](/commands/docs/overlay_use.md), [`hide`](/commands/docs/hide.md) ou [`source-env`](/commands/docs/source-env.md).
:::

#### Exemple : Générer du Code Source Dynamiquement

Considérons ce scénario :

```nu
("print Hello" | save output.nu;
source output.nu)
# => Error: nu::parser::sourced_file_not_found
# =>
# =>   × File not found
# =>    ╭─[entry #5:2:8]
# =>  1 │ "print Hello" | save output.nu
# =>  2 │ source output.nu
# =>    ·        ────┬────
# =>    ·            ╰── File not found: output.nu
# =>    ╰────
# =>   help: sourced files need to be available before your script is run
```

Le problème est le suivant :

1. La ligne 1 est parsée mais pas évaluée. En d'autres termes, `output.nu` n'est pas créé pendant l'étape de parsing, seulement pendant l'évaluation.
2. La ligne 2 est parsée. Comme `source` est un mot-clé du parser, la résolution du fichier inclus est tentée pendant le Parsing (étape 1). Mais `output.nu` n'existe pas encore ! S'il _existe_, ce n'est probablement pas le bon fichier ! Cela provoque l'erreur.

::: note
Taper ces deux lignes séparément dans le **_REPL_** fonctionnera, car la première ligne sera parsée et évaluée, puis la deuxième sera parsée et évaluée.

La limitation ne se produit que lorsque les deux sont parsées _ensemble_ comme une seule expression, ce qui peut faire partie d'un script, d'un bloc, d'un closure, ou d'une autre expression.

Voir la section [REPL](./how_nushell_code_gets_run.md#the-nushell-repl) dans _« Comment le code Nushell est exécuté »_ pour plus d'explications.
:::

#### Exemple : Créer Dynamiquement un Nom de Fichier à Inclure

Un autre scénario courant lors de la migration depuis un autre shell consiste à tenter de créer dynamiquement un nom de fichier à inclure :

```nu
let my_path = "~/nushell-files"
source $"($my_path)/common.nu"
# => Error:
# =>   × Error: nu::shell::not_a_constant
# =>   │
# =>   │   × Not a constant.
# =>   │    ╭─[entry #6:2:11]
# =>   │  1 │ let my_path = "~/nushell-files"
# =>   │  2 │ source $"($my_path)/common.nu"
# =>   │    ·           ────┬───
# =>   │    ·               ╰── Value is not a parse-time constant
# =>   │    ╰────
# =>   │   help: Only a subset of expressions are allowed constants during parsing. Try using the 'const' command or typing the value literally.
# =>   │
# =>    ╭─[entry #6:2:8]
# =>  1 │ let my_path = "~/nushell-files"
# =>  2 │ source $"($my_path)/common.nu"
# =>    ·        ───────────┬───────────
# =>    ·                   ╰── Encountered error during parse-time evaluation
# =>    ╰────
```

Comme l'assignation `let` n'est résolue qu'à l'évaluation, le mot-clé du parser `source` échouera pendant le parsing si on lui passe une variable.

::: details Comparaison avec Rust et C++
Imaginez que le code ci-dessus soit écrit dans un langage compilé typique comme C++ :

```cpp
#include <string>

std::string my_path("foo");
#include <my_path + "/common.h">
```

ou Rust :

```rust
let my_path = "foo";
use format!("{}::common", my_path);
```

Si vous avez déjà écrit un programme simple dans l'un de ces langages, vous voyez bien que ces exemples ne sont pas valides. Comme Nushell, les langages compilés exigent que tous les fichiers source soient prêts et disponibles pour le compilateur à l'avance.
:::

::: tip Voir Aussi
Comme indiqué dans le message d'erreur, cela peut fonctionner si `my_path` peut être défini comme une [constante](/book/variables#constant-variables), car les constantes sont résolues pendant le parsing.

```nu
const my_path = "~/nushell-files"
source $"($my_path)/common.nu"
```

Voir [Évaluation à Temps de Parsing des Constantes](./how_nushell_code_gets_run.md#parse-time-constant-evaluation) pour plus de détails.
:::

#### Exemple : Changer de Répertoire et Inclure un Fichier

Voici un dernier exemple — changer de répertoire puis tenter d'inclure un fichier dans ce répertoire.

```nu:line-numbers
if ('spam/foo.nu' | path exists) {
    cd spam
    source-env foo.nu
}
```

En vous appuyant sur ce que nous avons vu sur les étapes de Parsing/Évaluation de Nushell, essayez de repérer le problème dans cet exemple.

::: details Solution

À la ligne 3, pendant le Parsing, `source-env` tente de parser `foo.nu`. Cependant, `cd` ne s'exécute pas avant l'Évaluation. Cela entraîne une erreur au moment du parsing, car le fichier n'est pas trouvé dans le répertoire _courant_.

Pour résoudre cela, utilisez simplement le chemin complet du fichier à inclure :

```nu
    source-env spam/foo.nu
```

:::

### Résumé

::: important
Pour une explication plus approfondie de cette section, voir [Comment le code Nushell est exécuté](how_nushell_code_gets_run.md).
:::

::: warning Penser en Nushell
Nushell est conçu pour utiliser une étape de Parsing unique pour chaque expression ou fichier. Cette étape de Parsing a lieu avant et séparément de l'Évaluation. Bien que cela soit à la base de nombreuses fonctionnalités de Nushell, cela signifie aussi que les utilisateurs doivent comprendre les limitations que cela crée.
:::

## Les Variables sont Immuables par Défaut

Une autre surprise fréquente pour les utilisateurs venant d'autres langages est que les variables de Nushell sont immuables par défaut. Bien que Nushell dispose de variables mutables optionnelles, de nombreuses commandes de Nushell reposent sur un style de programmation fonctionnel qui requiert l'immuabilité.

Les variables immuables sont également essentielles à la [commande `par-each`](/commands/docs/par-each.md) de Nushell, qui permet d'opérer sur plusieurs valeurs en parallèle via des threads.

Consultez [Variables Immuables](variables.html#immutable-variables) et [Choisir entre variables mutables et immuables](variables.html#choosing-between-mutable-and-immutable-variables) pour plus d'informations.

::: warning Penser en Nushell
Si vous êtes habitué à vous appuyer sur des variables mutables, il vous faudra peut-être du temps pour réapprendre à coder dans un style plus fonctionnel. Nushell dispose de nombreuses fonctionnalités et commandes fonctionnelles qui opèrent sur et avec des variables immuables. Les apprendre vous aidera à écrire du code dans un style plus idiomatique Nushell.

Un bonus appréciable est le gain de performance que vous pouvez obtenir en exécutant des parties de votre code en parallèle avec `par-each`.
:::

## L'Environnement de Nushell est Scopé

Nushell s'inspire de nombreux principes des langages compilés. L'un d'eux est que les langages devraient éviter l'état global mutable. Les shells utilisent souvent la mutation globale pour mettre à jour l'environnement, mais Nushell tente d'éviter cette approche.

Dans Nushell, les blocs contrôlent leur propre environnement. Les modifications apportées à l'environnement sont limitées au bloc où elles se produisent.

En pratique, cela vous permet d'écrire un code plus concis pour travailler avec des sous-répertoires. Voici un exemple qui compile chaque sous-projet dans le répertoire courant :

```nu
ls | each { |row|
  cd $row.name
  make
}
```

La commande [`cd`](/commands/docs/cd.md) modifie la variable d'environnement `PWD`, mais ce changement ne survit pas à la fin du bloc. Cela permet à chaque itération de repartir du répertoire courant et d'entrer dans le sous-répertoire suivant.

Avoir un environnement scopé rend les commandes plus prévisibles, plus faciles à lire, et le moment venu, plus faciles à déboguer. C'est également une fonctionnalité clé pour la commande `par-each` mentionnée plus haut.

Nushell fournit également des commandes utilitaires comme [`load-env`](/commands/docs/load-env.md) pour charger plusieurs mises à jour d'environnement en une seule fois.

::: tip Voir Aussi
[Environnement - Scope](./environment.md#scoping)
:::

::: note
[`def --env`](/commands/docs/def.md) est une exception à cette règle. Elle vous permet de créer une commande qui modifie l'environnement du parent.
:::

::: warning Penser en Nushell
Utilisez l'environnement scopé pour écrire des scripts plus concis et éviter les mutations d'environnement global inutiles ou non souhaitées.
:::
