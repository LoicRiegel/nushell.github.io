# Complétions personnalisées

Les complétions personnalisées vous permettent de mélanger deux caractéristiques de Nushell : les commandes personnalisées et les complétions. Avec elles, vous êtes en mesure de créer des commandes qui gèrent les complétions pour les paramètres positionnels et les paramètres de flag. Ces complétions personnalisées fonctionnent à la fois pour les [commandes personnalisées](custom_commands.md) et les [commandes externes connus, ou `extern`,](externs.md).

Une complétation est définie en deux étapes :

- Définir une commande de complétation (a.k.a. completer) qui retourne les valeurs possibles à suggérer
- Attacher le completer à l'annotation de type (shape) d'un argument de commande autre en utilisant `<shape>@<completer>`

Voici un exemple simple :

```nu
# Commande de complétation
def animals [] { ["cat", "dog", "eel" ] }
# Commande à compléter
def my-command [animal: string@animals] { print $animal }
my-command
# => cat                 dog                 eel
```

La première ligne définit une commande personnalisée qui retourne une liste de trois animaux différents. Ce sont les valeurs possibles pour la complétation.

::: tip
Pour supprimer les complétions pour un argument (par exemple, un `int` qui peut accepter n'importe quel entier), définissez un completer qui retourne une liste vide (`[ ]`).
:::

Dans la deuxième ligne, `string@animals` dit à Nushell deux choses — la forme de l'argument pour la vérification de type et le completer qui suggérera les valeurs possibles pour l'argument.

La troisième ligne est une démonstration de la complétation. Tapez le nom de la commande personnalisée `my-command`, suivi d'un espace, puis appuyez sur la touche <kbd>Tab</kbd>. Cela affiche un menu avec les complétions possibles. Les complétions personnalisées fonctionnent de la même manière que les autres complétions du système, vous permettant de taper `e` suivi de la touche <kbd>Tab</kbd> pour compléter automatiquement « eel ».

::: tip
Quand le menu de complétation est affiché, l'invite change pour inclure le caractère `|` par défaut. Pour modifier le marqueur d'invite, modifiez la valeur `marker` de l'enregistrement, où la clé `name` est `completion_menu`, dans la liste `$env.config.menus`. Voir aussi [la configuration du menu de complétation](/book/line_editor.md#completion-menu).
:::

::: tip
Pour revenir aux complétions de fichier intégrées de Nushell, retournez `null` plutôt qu'une liste de suggestions.
:::

## Options pour les complétions personnalisées

Si vous voulez choisir comment vos complétions sont filtrées et triées, vous pouvez aussi retourner un enregistrement plutôt qu'une liste. La liste des suggestions de complétation doit être sous la clé `completions` de cet enregistrement. En option, il peut aussi avoir, sous la clé `options`, un enregistrement contenant les paramètres optionnels suivants :

- `sort` - Définissez ceci à `false` pour arrêter Nushell du tri de vos complétions. Par défaut, cela est `true`, et les complétions sont triées selon `$env.config.completions.sort`.
- `case_sensitive` - Réglez sur `true` pour que les complétions personnalisées soient mises en correspondance de manière sensible à la casse, `false` sinon. Utilisé pour remplacer `$env.config.completions.case_sensitive`.
- `completion_algorithm` - Réglez ceci sur `prefix`, `substring`, ou `fuzzy` pour choisir comment vos complétions sont mises en correspondance avec le texte dactylographié. Utilisé pour remplacer `$env.config.completions.algorithm`.

Voici un exemple montrant comment définir ces options :

```nu
def animals [] {
    {
        options: {
            case_sensitive: false,
            completion_algorithm: substring,
            sort: false,
        },
        completions: [cat, rat, bat]
    }
}
def my-command [animal: string@animals] { print $animal }
```

Maintenant, si vous essayez de compléter `A`, vous obtenez les complétions suivantes :

```nu
>| my-command A
cat                 rat                 bat
```

Parce que nous avons rendu la correspondance insensible à la casse, Nushell trouvera la sous-chaîne « a » dans toutes les suggestions de complétation. De plus, parce que nous avons défini `sort: false`, les complétions seront laissées dans leur ordre d'origine. Ceci est utile si vos complétions sont déjà triées dans un ordre particulier sans rapport avec leur texte (par ex. par date).

## Modules et complétions personnalisées

Étant donné que les commandes de complétation ne sont pas destinées à être appelées directement, il est courant de les définir dans des modules.

En étendant l'exemple ci-dessus avec un module :

```nu
module commands {
    def animals [] {
        ["cat", "dog", "eel" ]
    }

    export def my-command [animal: string@animals] {
        print $animal
    }
}
```

Dans ce module, seule la commande personnalisée `my-command` est exportée. La complétation `animals` n'est pas exportée. Cela permet aux utilisateurs de ce module d'appeler la commande et même d'utiliser la logique de complétation personnalisée sans avoir accès à la commande de complétation elle-même. Ceci résulte en une API plus propre et plus maintenable.

::: tip
Les completers sont attachés aux commandes personnalisées en utilisant `@` au moment de l'analyse. Cela signifie que, pour qu'un changement à la commande de complétation prenne effet, la commande personnalisée publique doit aussi être réanalysée. Importer un module satisfait les deux exigences en même temps avec une seule instruction `use`.
:::

## Complétions personnalisées sensibles au contexte

Il est possible de passer le contexte à la commande de complétation. C'est utile dans les situations où il est nécessaire de connaître les arguments ou flags précédents pour générer des complétions précises.

En appliquant ce concept à l'exemple précédent :
