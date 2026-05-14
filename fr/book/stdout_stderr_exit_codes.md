# Stdout, Stderr et codes de sortie

Une part importante de l'interopérabilité entre Nushell et les commandes externes est de travailler avec les flux de données standards provenant de l'externe.

Le premier de ces flux importants est stdout.

## Stdout

Stdout est le moyen par lequel la plupart des applications externes enverront les données dans le pipeline ou à l'écran. Les données envoyées par une application externe à son stdout sont reçues par Nushell par défaut si c'est une partie d'un pipeline :

```nu
external | str join
```

Le ci-dessus appellerait l'externe nommé `external` et redirigerait le flux de sortie stdout dans le pipeline. Avec cette redirection, Nushell peut alors transmettre les données à la commande suivante dans le pipeline, ici [`str join`](/commands/docs/str_join.md).

Sans le pipeline, Nushell n'effectuera aucune redirection, lui permettant d'imprimer directement à l'écran.

## Stderr

Un autre flux courant que les applications externes utilisent souvent pour imprimer les messages d'erreur est stderr. Par défaut, Nushell ne fait aucune redirection de stderr, ce qui signifie que par défaut, il imprimera à l'écran.

Mais vous pouvez passer stderr à une commande ou un fichier si vous voulez :

- utiliser `e>|` pour passer stderr à la commande suivante.
- utiliser `e> file` pour rediriger stderr vers un fichier.
- utiliser `do -i { cmd } | complete` pour capturer le message stderr.

## Code de sortie

Enfin, les commandes externes ont un « code de sortie ». Ces codes aident à donner un indice à l'appelant si la commande s'est exécutée avec succès.

Nushell suit le dernier code de sortie de l'externe récemment complété de l'une des deux façons. La première façon est avec la variable d'environnement `LAST_EXIT_CODE`.

```nu
do { external }
$env.LAST_EXIT_CODE
```

La deuxième façon est d'utiliser la commande [`complete`](/commands/docs/complete.md).

## Utilisation de la commande [`complete`](/commands/docs/complete.md)

La commande [`complete`](/commands/docs/complete.md) vous permet d'exécuter une commande externe jusqu'à la fin et de rassembler stdout, stderr et le code de sortie ensemble dans un seul enregistrement.

Si nous essayons d'exécuter l'externe `cat` sur un fichier qui n'existe pas, nous pouvons voir ce que [`complete`](/commands/docs/complete.md) fait avec les flux, y compris stderr redirigé :

```nu
cat unknown.txt | complete
# => ╭───────────┬─────────────────────────────────────────────╮
```
