# Alias

Les alias dans Nushell offrent un moyen de faire un simple remplacement d'appels de commande (commandes externes et internes). Cela vous permet de créer un nom court pour une commande plus longue, y compris ses arguments par défaut.

Par exemple, créons un alias appelé `ll` qui se développera en `ls -l`.

```nu
alias ll = ls -l
```

Nous pouvons maintenant appeler cet alias :

```nu
ll
```

Une fois que nous le faisons, c'est comme si nous avions tapé `ls -l`. Cela nous permet également de passer des flags ou des paramètres positionnels. Par exemple, nous pouvons maintenant aussi écrire :

```nu
ll -a
```

Et obtenir l'équivalent d'avoir tapé `ls -l -a`.

## Lister tous les alias chargés

Vos alias utilisables peuvent être vus dans `scope aliases` et `help aliases`.

## Persistance

Pour rendre vos alias permanents, ils doivent être ajoutés à votre fichier _config.nu_ en exécutant `config nu` pour ouvrir un éditeur et les insérer, puis en redémarrant nushell.
par ex. avec l'alias `ll` ci-dessus, vous pouvez ajouter `alias ll = ls -l` n'importe où dans _config.nu_

```nu
$env.config = {
    # configuration principale
}

alias ll = ls -l

# autre configuration et chargement de script
```

## Piping dans les alias

Notez que `alias uuidgen = uuidgen | tr A-F a-f` (pour que uuidgen sur mac se comporte comme linux) ne fonctionnera pas.
La solution est de définir une commande sans paramètres qui appelle le programme système `uuidgen` via `^`.

```nu
def uuidgen [] { ^uuidgen | tr A-F a-f }
```

Voir plus dans la section [commandes personnalisées](custom_commands.md) de ce livre.

Ou un exemple plus idiomatique avec les commandes internes de nushell

```nu
def lsg [] { ls | sort-by type name -i | grid -c | str trim }
```

affichant tous les fichiers et dossiers listés dans une grille.

## Remplacer les commandes existantes à l'aide d'alias

::: warning Attention !
Lors du remplacement de commandes, il est préférable de « sauvegarder » d'abord la commande et d'éviter les erreurs de récursion.
:::

Comment sauvegarder une commande comme `ls` :

```nu
alias core-ls = ls    # Cela créera un nouvel alias core-ls pour ls
```

Maintenant vous pouvez utiliser `core-ls` comme `ls` dans votre programmation nu. Vous verrez plus bas comment utiliser `core-ls`.

La raison pour laquelle vous devez utiliser alias est que, contrairement à `def`, les alias dépendent de la position. Donc, vous devez d'abord « sauvegarder » l'ancienne commande avec un alias, avant de la redéfinir.
Si vous ne sauvegardez pas la commande et que vous la remplacez à l'aide de `def`, vous obtenez une erreur de récursion.

```nu
def ls [] { ls }; ls    # Ne faites PAS cela ! Cela lancera une erreur de récursion

#sortie:
#Error: nu::shell::recursion_limit_reached
#
#  × Limite de récursion (50) atteinte
#     ╭─[C:\Users\zolodev\AppData\Roaming\nushell\config.nu:807:1]
# 807 │
# 808 │ def ls [] { ls }; ls
#     ·           ───┬──
#     ·              ╰── Cela s'est appelé lui-même trop de fois
#     ╰────
```

La façon recommandée de remplacer une commande existante est de faire de l'ombrage de commande.
Voici un exemple d'ombrage de la commande `ls`.

```nu
# alias la commande ls intégrée à ls-builtins
alias ls-builtin = ls

# Listez les noms de fichiers, les tailles et les heures de modification des éléments d'un répertoire.
def ls [
    --all (-a),         # Afficher les fichiers cachés
    --long (-l),        # Obtenez toutes les colonnes disponibles pour chaque entrée (plus lent ; les colonnes dépendent de la plateforme)
    --short-names (-s), # Imprimez uniquement les noms de fichiers, et non le chemin
    --full-paths (-f),  # afficher les chemins comme chemins absolus
    --du (-d),          # Afficher la taille apparente du répertoire (« utilisation disque ») à la place de la taille de métadonnées du répertoire
    --directory (-D),   # Lister le répertoire spécifié lui-même au lieu de son contenu
    --mime-type (-m),   # Afficher le type mime dans la colonne type au lieu de 'fichier' (basé sur les noms de fichiers uniquement ; le contenu des fichiers n'est pas examiné)
    --threads (-t),     # Utiliser plusieurs threads pour lister le contenu. La sortie sera non-déterministe.
    ...pattern: glob,   # Le motif glob à utiliser.
]: [ nothing -> table ] {
    let pattern = if ($pattern | is-empty) { [ '.' ] } else { $pattern }
    (ls-builtin
        --all=$all
        --long=$long
        --short-names=$short_names
        --full-paths=$full_paths
        --du=$du
        --directory=$directory
        --mime-type=$mime_type
        --threads=$threads
        ...$pattern
    ) | sort-by type name -i
}
```
