# Externs

Utiliser des commandes externes (aussi appelés binaires ou applications) est une caractéristique fondamentale de tout shell. Nushell permet aux commandes personnalisées de tirer parti de nombreuses de ses fonctionnalités, telles que :

- Vérification des types au moment de l'analyse
- Complétions
- Surbrillance de syntaxe

Le support de ces fonctionnalités est fourni en utilisant le mot-clé `extern`, qui permet une signature complète d'être définie pour les commandes externes.

Voici un court exemple pour la commande `ssh` :

```nu
module "ssh extern" {
  def complete_none [] { [] }

  def complete_ssh_identity [] {
    ls ~/.ssh/id_*
    | where {|f|
        ($f.name | path parse | get extension) != "pub"
      }
    | get name
  }

  export extern ssh [
    destination?: string@complete_none  # Destination Host
    -p: int                             # Destination Port
    -i: string@complete_ssh_identity    # Identity File
  ]
}
use "ssh extern" ssh
```

Notez que la syntaxe ici est similaire à celle du mot-clé `def` lors de la définition d'une commande personnalisée. Vous pouvez décrire les flags, les paramètres positionnels, les types, les completers et plus encore.

Cette implémentation :

- Fournira `-p` et `-i` (avec descriptions) comme complétions possibles pour `ssh -`.
- Effectuera la vérification des types au moment de l'analyse. Tenter d'utiliser un non-`int` pour le numéro de port entraînera une erreur (et une surbrillance de syntaxe de condition d'erreur).
- Offrira une surbrillance de syntaxe au moment de l'analyse en fonction des formes des arguments.
- Offrira tous les fichiers de clé privée dans `~/.ssh` comme valeurs de complétation pour l'option `-i` (identité)
- N'offrira pas de complétions pour l'hôte de destination. Sans un completer qui retourne une liste vide, Nushell tenterait d'utiliser le completer « Fichier » par défaut.

  Voir le [Dépôt Nu_scripts](https://github.com/nushell/nu_scripts/blob/main/custom-completions/ssh/ssh-completions.nu) pour une implémentation qui récupère les hôtes à partir des fichiers de configuration SSH.

::: tip Remarque
Un commentaire Nushell qui continue sur la même ligne à des fins de documentation d'argument nécessite un espace avant le signe `#`.
:::

## Spécificateurs de format

Les paramètres positionnels peuvent être rendus optionnels avec un `?` (comme vu ci-dessus). Les paramètres restants (`rest`) peuvent être appariés avec `...` avant le nom du paramètre. Par exemple :

```nu
export extern "git add" [
  ...pathspecs: path
  # …
]
```

## Limitations

Il y a quelques limitations à la syntaxe `extern` actuelle. Dans Nushell, les flags et les arguments positionnels sont très flexibles — les flags peuvent précéder les arguments positionnels, les flags peuvent être mélangés avec les arguments positionnels et les flags peuvent suivre les arguments positionnels. De nombreuses commandes externes ne sont pas aussi flexibles. Il n'y a pas encore moyen d'exiger un ordre particulier des flags et des arguments positionnels à la manière requise par l'externe.

La deuxième limitation est que certaines externes exigent que les flags soient passés en utilisant `=` pour séparer le flag et la valeur. Dans Nushell, `=` est une syntaxe optionnelle pratique et il n'y a actuellement aucun moyen d'exiger son utilisation.

De plus, les externes appelées via le signe caret (par ex., `^ssh`) ne sont pas reconnues par `extern`.

Enfin, certaines commandes externes supportent les arguments `-long` en utilisant un tiret de début unique. La syntaxe `extern` de Nushell ne peut pas encore représenter ces arguments.
