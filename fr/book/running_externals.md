# Exécution de commandes système (externes)

Nu fournit un ensemble de commandes que vous pouvez utiliser sur différents systèmes d'exploitation (commandes « internes ») et avoir cette cohérence est utile lors de la création de code multi-plateforme. Parfois, cependant, vous voulez exécuter une commande externe qui a le même nom qu'une commande interne de Nushell. Pour exécuter la commande [`ls`](/commands/docs/ls.md) ou [`date`](/commands/docs/date.md) externe, par exemple, préfacez-la avec le sigil caret (^). Préfacer avec le caret appelle la commande externe trouvée dans le `PATH` de l'utilisateur (par exemple `/bin/ls`) au lieu de la commande interne [`ls`](/commands/docs/ls.md) de Nushell).

Commande interne Nushell :

```nu
ls
```

Commande externe (généralement `/usr/bin/ls`) :

```nu
^ls
```

::: note
Sur Windows, `ls` est un _alias_ PowerShell par défaut, donc `^ls` ne trouvera pas de commande système correspondante.
:::

## Notes supplémentaires pour Windows

Lors de l'exécution d'une commande externe sur Windows,
Nushell transfère certaines commandes internes `CMD.EXE` à cmd au lieu d'essayer d'exécuter des commandes externes.
[Venir de CMD.EXE](coming_from_cmd.md) contient une liste de ces commandes et décrit le comportement en plus de détails.
