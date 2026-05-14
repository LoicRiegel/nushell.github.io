# Piles de répertoires

Comme certains autres shells, Nushell fournit une fonction de pile de répertoires pour basculer facilement entre plusieurs répertoires. Dans Nushell, cette fonction fait partie de la [Bibliothèque standard](./standard_library.md) et peut être consultée de plusieurs façons.

::: note
Dans Nushell, la « pile » est représentée comme une `list`, mais la fonctionnalité globale est similaire à celle d'autres shells.
:::

[[toc]]

## Module `dirs` et commandes

Pour utiliser la commande `dirs` et ses sous-commandes, importez d'abord le module en utilisant :

```nu
use std/dirs
```

::: tip
Pour rendre la fonction disponible chaque fois que vous démarrez Nushell, ajoutez la commande ci-dessus à votre [configuration de démarrage](./configuration.md).
:::

Cela met plusieurs nouvelles commandes à disposition :

| Commande    | Description                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dirs`      | Liste les répertoires de la pile                                                                                                                                           |
| `dirs add`  | Ajoute un ou plusieurs répertoires à la liste. Le premier répertoire répertorié devient le nouveau répertoire actif. Similaire à la commande `pushd` dans d'autres shells. |
| `dirs drop` | Supprime le répertoire courant de la liste. Le répertoire précédent de la liste devient le nouveau répertoire actif. Similaire à la commande `popd` dans d'autres shells.  |
| `dirs goto` | Saute vers le répertoire en utilisant son index dans la liste                                                                                                              |
| `dirs next` | Rend le répertoire suivant de la liste le répertoire actif. Si le répertoire actif courant est le dernier de la liste, alors cyclez jusqu'au début de la liste.            |
| `dirs prev` | Rend le répertoire précédent de la liste le répertoire actif. Si le répertoire actif courant est le premier de la liste, alors cyclez jusqu'à la fin de la liste.          |

Quand nous commençons à utiliser `dirs`, il n'y a qu'un seul répertoire dans la liste, celui qui est actif. Vous pouvez, comme toujours, modifier ce répertoire en utilisant la commande `cd`.

```nu
cd ~
use std/dirs
dirs
# => ╭───┬────────┬─────────────────────────────────╮
# => │ # │ active │              path               │
# => ├───┼────────┼─────────────────────────────────┤
# => │ 0 │ true   │ /home/myuser                    │
# => ╰───┴────────┴─────────────────────────────────╯

cd ~/src/repo/nushell
dirs
# => ╭───┬────────┬─────────────────────────────────╮
# => │ # │ active │              path               │
# => ├───┼────────┼─────────────────────────────────┤
# => │ 0 │ true   │ /home/myuser/repo/nushell       │
# => ╰───┴────────┴─────────────────────────────────╯
```

Notez que `cd` change seulement le répertoire actif.

Pour _ajouter_ le répertoire courant à la liste, basculez vers un nouveau répertoire actif en utilisant la commande `dirs add` :

```nu
dirs add ../reedline
dirs
# => ╭───┬────────┬──────────────────────────────────╮
# => │ # │ active │               path               │
# => ├───┼────────┼──────────────────────────────────┤
# => │ 0 │ false  │ /home/myuser/src/repo/nushell    │
# => │ 1 │ true   │ /home/myuser/src/repo/reedline   │
# => ╰───┴────────┴──────────────────────────────────╯
```

Continuons et ajoutons quelques répertoires couramment utilisés à la liste :

```nu
dirs add ../nu_scripts
dirs add ~
dirs
# => ╭───┬────────┬────────────────────────────────────╮
# => │ # │ active │                path                │
# => ├───┼────────┼────────────────────────────────────┤
# => │ 0 │ false  │ /home/myuser/src/repo/nushell      │
# => │ 1 │ false  │ /home/myuser/src/repo/reedline     │
# => │ 2 │ false  │ /home/myuser/src/repo/nu_scripts   │
# => │ 3 │ true   │ /home/myuser                       │
# => ╰───┴────────┴────────────────────────────────────╯
```

Nous pouvons maintenant basculer facilement entre eux en utilisant `dirs next`, `dirs prev` ou `dirs goto` :

```nu
dirs next
# Active était 3, est maintenant 0
pwd
# => /home/myuser/src/repo/nushell
dirs goto 2
# => /home/myuser/src/repo/nu_scripts
```

Quand vous avez terminé votre travail dans un répertoire, vous pouvez le supprimer de la liste en utilisant :

```nu
dirs drop
```
