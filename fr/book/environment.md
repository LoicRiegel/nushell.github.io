# Environnement

Une tâche courante dans un shell est de contrôler l'environnement que les applications externes utiliseront. C'est souvent fait automatiquement, car l'environnement est emballé et donné à l'application externe au fur et à mesure qu'elle se lance. Parfois, cependant, nous voulons avoir un contrôle plus précis sur les variables d'environnement qu'une application voit.

Vous pouvez voir les variables d'environnement courantes dans la variable $env :

```nu
$env | table -e
# => ╭──────────────────────────────────┬───────────────────────────────────────────╮
# => │                                  │ ╭──────┬────────────────────────────────╮ │
# => │ ENV_CONVERSIONS                  │ │      │ ╭─────────────┬──────────────╮ │ │
# => │                                  │ │ PATH │ │ from_string │ <Closure 32> │ │ │
# => │                                  │ │      │ │ to_string   │ <Closure 34> │ │ │
# => │                                  │ │      │ ╰─────────────┴──────────────╯ │ │
# => │                                  │ │      │ ╭─────────────┬──────────────╮ │ │
# => │                                  │ │ Path │ │ from_string │ <Closure 36> │ │ │
# => │                                  │ │      │ │ to_string   │ <Closure 38> │ │ │
# => │                                  │ │      │ ╰─────────────┴──────────────╯ │ │
# => │                                  │ ╰──────┴────────────────────────────────╯ │
# => │ HOME                             │ /Users/jelle                              │
# => │ LSCOLORS                         │ GxFxCxDxBxegedabagaced                    │
# => | ...                              | ...                                       |
# => ╰──────────────────────────────────┴───────────────────────────────────────────╯
```

Dans Nushell, les variables d'environnement peuvent être n'importe quelle valeur et avoir n'importe quel type. Vous pouvez voir le type d'une variable d'env avec la commande describe, par exemple : `$env.PROMPT_COMMAND | describe`.

Pour envoyer des variables d'environnement aux applications externes, les valeurs devront être converties en chaînes. Voir [Conversions de variables d'environnement](#conversions-de-variables-d-environnement) sur le fonctionnement de ceci.

L'environnement est initialement créé à partir des [fichiers de configuration](configuration.md) et de l'environnement dans lequel Nu est exécuté.

## Définir les variables d'environnement

Il y a plusieurs façons de définir une variable d'environnement :

### Assignation $env.VAR

Utiliser `$env.VAR = "val"` est la méthode la plus directe

```nu
$env.FOO = 'BAR'
```

Donc, si vous voulez étendre la variable Windows `Path`, par exemple, vous pourriez faire cela comme suit.

```nu
$env.Path = ($env.Path | prepend 'C:\path\you\want\to\add')
```

Ici, nous avons ajouté notre dossier avant les dossiers existants dans le Path, afin qu'il ait la priorité la plus élevée.
Si vous voulez lui donner la priorité la plus basse à la place, vous pouvez utiliser la commande [`append`](/commands/docs/append.md).

### [`load-env`](/commands/docs/load-env.md)

Si vous avez plus d'une variable d'environnement que vous aimeriez définir, vous pouvez utiliser [`load-env`](/commands/docs/load-env.md) pour créer un tableau de paires nom/valeur et charger plusieurs variables à la fois :

```nu
load-env { "BOB": "FOO", "JAY": "BAR" }
```

### Variables d'environnement ponctuelles

Celles-ci sont définies pour être actives uniquement temporairement pendant la durée de l'exécution d'un bloc de code.
Voir [Variables d'environnement à usage unique](environment.md#variables-d-environnement-à-usage-unique) pour plus de détails.

### Appel d'une commande définie avec [`def --env`](/commands/docs/def.md)

Voir [Définir l'environnement à partir de commandes personnalisées](custom_commands.md#changing-the-environment-in-a-custom-command) pour plus de détails.

### Utilisation des exports de modules

Voir [Modules](modules.md) pour plus de détails.

## Lire les variables d'environnement

Les variables d'environnement individuelles sont des champs d'un enregistrement stocké dans la variable `$env` et peuvent être lus avec `$env.VARIABLE` :

```nu
$env.FOO
# => BAR
```

Parfois, vous pourriez vouloir accéder à une variable d'environnement qui pourrait ne pas être définie. Envisagez d'utiliser l'[opérateur point d'interrogation](types_of_data.md#chemins-de-cellule-optionnels) pour éviter une erreur :

```nu
$env.FOO | describe
# => Error: nu::shell::column_not_found
# =>
# =>   × Cannot find column
# =>    ╭─[entry #1:1:1]
# =>  1 │ $env.FOO
# =>    · ──┬─ ─┬─
# =>    ·   │   ╰── cannot find column 'FOO'
# =>    ·   ╰── value originates here
# =>    ╰────

$env.FOO? | describe
# => nothing

$env.FOO? | default "BAR"
```
