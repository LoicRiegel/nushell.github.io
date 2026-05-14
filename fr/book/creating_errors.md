# Créer vos propres erreurs

Utilisez l'information de [métadonnée](metadata.md), vous pouvez créer vos propres messages d'erreur personnalisés. Les messages d'erreur sont composés de plusieurs parties :

- Le titre de l'erreur
- L'étiquette du message d'erreur, qui inclut à la fois le texte de l'étiquette et l'intervalle à souligner

Vous pouvez utiliser la commande [`error make`](/commands/docs/error_make.md) pour créer vos propres messages d'erreur. Par exemple, supposons que vous ayiez votre propre commande appelée `my-command` et que vous vouliez donner une erreur à l'appelant à propos de quelque chose de mal avec un paramètre qui a été passé.

Tout d'abord, vous pouvez prendre l'intervalle du lieu d'où l'argument provient :

```nu
let span = (metadata $x).span;
```

Ensuite, vous pouvez créer une erreur en utilisant la commande [`error make`](/commands/docs/error_make.md). Cette commande prend un enregistrement qui décrit l'erreur à créer :

```nu
error make {msg: "this is fishy", label: {text: "fish right here", span: $span } }
```

Combiné avec votre commande personnalisée, cela pourrait ressembler à ceci :

```nu
def my-command [x] {
    let span = (metadata $x).span;
    error make {
        msg: "this is fishy",
        label: {
            text: "fish right here",
            span: $span
        }
    }
}
```

Quand elle est appelée avec une valeur, nous verrons maintenant un message d'erreur retourné :

```nu
my-command 100
# => Error:
# =>   × this is fishy
# =>    ╭─[entry #5:1:1]
# =>  1 │ my-command 100
# =>    ·            ─┬─
# =>    ·             ╰── fish right here
# =>    ╰────
```
