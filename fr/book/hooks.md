# Hooks

Les hooks vous permettent d'exécuter un extrait de code dans certaines situations prédéfinies.
Ils ne sont disponibles que dans le mode interactif ([REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)), ils ne fonctionnent pas si vous exécutez Nushell avec un script (`nu script.nu`) ou l'argument de commande (`nu -c "print foo"`).

Actuellement, nous supportons ces types de hooks :

- `pre_prompt` : Déclenché avant que l'invite ne soit dessinée
- `pre_execution` : Déclenché avant que la ligne d'entrée commence à s'exécuter
- `env_change` : Déclenché quand une variable d'environnement change
- `display_output` : Un bloc auquel la sortie est transmise
- `command_not_found` : Déclenché quand une commande n'est pas trouvée

Pour clarifier, nous pouvons décomposer le cycle d'exécution de Nushell.
Les étapes pour évaluer une ligne en mode REPL sont les suivantes :

1. Vérifier les hooks `pre_prompt` et les exécuter
1. Vérifier les hooks `env_change` et les exécuter
1. Afficher l'invite et attendre l'entrée de l'utilisateur
1. Après que l'utilisateur ait tapé quelque chose et appuyé sur « Entrée » : Vérifier les hooks `pre_execution` et les exécuter
1. Analyser et évaluer l'entrée de l'utilisateur
1. Si une commande n'est pas trouvée : Exécuter le hook `command_not_found`. S'il retourne une chaîne, l'afficher.
1. Si `display_output` est défini, l'utiliser pour imprimer la sortie de la commande
1. Revenir à 1.

## Hooks de base

Pour activer les hooks, définissez-les dans votre [configuration](configuration.md) :

```nu
$env.config.hooks = {
    pre_prompt: [{ print "pre prompt hook" }]
    pre_execution: [{ print "pre exec hook" }]
    env_change: {
        PWD: [{|before, after| print $"changing directory from ($before) to ($after)" }]
    }
}
```

Essayez de mettre le ci-dessus dans votre configuration, exécutez Nushell et naviguez dans votre système de fichiers.
Quand vous changez un répertoire, la variable d'environnement `PWD` change et le changement déclenche le hook avec les valeurs précédentes et courantes stockées dans les variables `before` et `after`, respectivement.

Au lieu de définir juste un seul hook par déclencheur, il est possible de définir une **liste de hooks** qui s'exécuteront en séquence :

```nu
$env.config.hooks = {
    pre_prompt: [
        { print "pre prompt hook" }
        { print "pre prompt hook2" }
    ]
    pre_execution: [
        { print "pre exec hook" }
        { print "pre exec hook2" }
    ]
    env_change: {
        PWD: [
            {|before, after| print $"changing directory from ($before) to ($after)" }
            {|before, after| print $"changing directory from ($before) to ($after) 2" }
        ]
    }
}
```

Au lieu de remplacer tous les hooks, vous pouvez ajouter un nouveau hook à la configuration existante :

```nu
$env.config.hooks.pre_execution = $env.config.hooks.pre_execution | append { print "pre exec hook3" }
```

## Modification de l'environnement

Une caractéristique des hooks est qu'ils préservent l'environnement.
Les variables d'environnement définies à l'intérieur du bloc **hook** seront préservées de la même manière que [`def --env`](environment.md#defining-environment-from-custom-commands).
Vous pouvez le tester avec l'exemple suivant :

```nu
$env.config = ($env.config | upsert hooks {
    pre_prompt: { $env.SPAM = "eggs" }
})
```
