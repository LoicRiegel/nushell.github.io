# `explore`

Explore est un paginateur de tableau, tout comme `less` mais pour les données structurées de tableau.

## Signature

`> explore --head --index --reverse --peek`

### Paramètres

- `--head {bool}`: Afficher ou masquer les en-têtes de colonne (défaut true)
- `--index, -i`: Afficher les index des lignes lors de la visualisation d'une liste
- `--tail, -t`: Commencer avec la fenêtre d'affichage défilée jusqu'en bas
- `--peek, -p`: Lors de la fermeture, afficher la valeur de la cellule sur laquelle le curseur était

## Commencer

```nu
ls | explore -i
```

![explore-ls-png](https://user-images.githubusercontent.com/20165848/207849604-421312e3-537f-4b2e-b83e-f1f83f2a79d5.png)

Donc le point principal de [`explore`](/commands/docs/explore.md) est `:table` (que vous voyez dans la capture d'écran ci-dessus).

Vous pouvez interagir avec cela via les touches `<Left>`, `<Right>`, `<Up>`, `<Down>` des _flèches_. Il supporte également les liaisons clavier `Vim` `<h>`, `<j>`, `<k>` et `<l>`, `<Ctrl-f>` et `<Ctrl-b>`, et il supporte les liaisons clavier `Emacs` `<Ctrl-v>`, `<Alt-v>`, `<Ctrl-p>` et `<Ctrl-n>`.

Vous pouvez inspecter les valeurs sous-jacentes en entrant en mode curseur. Vous pouvez appuyer soit sur `<i>` soit sur `<Enter>` pour le faire.
Ensuite, en utilisant les touches _flèche_, vous pouvez choisir une cellule nécessaire.
Et vous serez en mesure de voir sa structure sous-jacente.

Vous pouvez obtenir plus d'informations sur les divers aspects de cela par `:help`.

## Commandes

[`explore`](/commands/docs/explore.md) a une liste de commandes intégrées que vous pouvez utiliser. Les commandes sont exécutées en appuyant sur `<:>` puis un nom de commande.

Pour découvrir la liste complète des commandes, vous pouvez taper `:help`.

## Config

Vous pouvez configurer de nombreuses choses (y compris les styles et les couleurs), via la configuration.
Vous pouvez trouver un exemple de configuration dans [`default-config.nu`](https://github.com/nushell/nushell/blob/main/crates/nu-utils/src/default_files/default_config.nu).

## Exemples

### Jeter un coup d'œil à une valeur

```nu
$nu | explore --peek
```

![explore-peek-gif](https://user-images.githubusercontent.com/20165848/207854897-35cb7b1d-7f7d-4ae2-9ec8-df19ac04ac99.gif)

### Commande `:try`

Il y a un environnement interactif que vous pouvez utiliser pour naviguer dans les données en utilisant `nu`.

![explore-try-gif](https://user-images.githubusercontent.com/20165848/208159049-0954c327-9cdf-4cb3-a6e9-e3ba86fde55c.gif)

#### Garder la valeur choisie par `$nu`

N'oubliez pas que vous pouvez le combiner avec `--peek`.

![explore-try-nu-gif](https://user-images.githubusercontent.com/20165848/208161203-96b51209-726d-449a-959a-48b205c6f55a.gif)
