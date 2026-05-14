# Travailler avec les chaînes

Comme dans la plupart des langages, les chaînes sont une collection de 0 ou plus de caractères qui représentent du texte. Cela peut inclure les noms de fichiers, les chemins de fichiers, les noms de colonnes,
et bien plus. Les chaînes sont tellement courantes que Nushell offre plusieurs formats de chaîne pour correspondre à votre cas d'usage :

## Formats de chaîne en un coup d'œil

| Format de chaîne                                                | Exemple                 | Échappements            | Notes                                                                                         |
| --------------------------------------------------------------- | ----------------------- | ----------------------- | --------------------------------------------------------------------------------------------- |
| [Chaîne entre guillemets simples](#single-quoted-strings)       | `'[^\n]+'`              | Aucun                   | Ne peut pas contenir de guillemets simples dans la chaîne                                     |
| [Chaîne entre guillemets doubles](#double-quoted-strings)       | `"The\nEnd"`            | Échappements de style C | Tous les antislashs littéraux doivent être échappés                                           |
| [Chaînes brutes](#raw-strings)                                  | `r#'Raw string'#`       | Aucun                   | Peut inclure des guillemets simples                                                           |
| [Chaîne de mot nu](#bare-word-strings)                          | `ozymandias`            | Aucun                   | Ne peut contenir que des caractères « mot »; Ne peut pas être utilisé en position de commande |
| [Chaîne avec backtick](#backtick-quoted-strings)                | <code>\`[^\n]+\`</code> | Aucun                   | Chaîne nue qui peut inclure des espaces. Ne peut contenir aucun backtick                      |
| [Interpolation entre guillemets simples](#string-interpolation) | `$'Captain ($name)'`    | Aucun                   | Ne peut contenir aucun `'` ou `()` non apparié                                                |
| [Interpolation entre guillemets doubles](#string-interpolation) | `$"Captain ($name)"`    | Échappements de style C | Tous les antislashs littéraux et `()` doivent être échappés                                   |

## Chaînes entre guillemets simples

La chaîne la plus simple en Nushell est la chaîne entre guillemets simples. Cette chaîne utilise le caractère `'` pour entourer du texte. Voici le texte pour hello world comme une chaîne entre guillemets simples :

```nu
'hello world'
# => hello world
'The
end'
# => The
# => end
```

Les chaînes entre guillemets simples ne font rien au texte qu'on leur donne, les rendant idéales pour contenir une large gamme de données textuelles.

## Chaînes entre guillemets doubles

Pour les chaînes plus complexes, Nushell offre également des chaînes entre guillemets doubles. Ces chaînes utilisent le caractère `"` pour entourer le texte. Elles soutiennent également la possibilité d'échapper les caractères dans le texte en utilisant le caractère `\`.

Par exemple, nous pourrions écrire le texte hello suivi d'une nouvelle ligne puis world, en utilisant des caractères d'échappement et une chaîne entre guillemets doubles :

```nu
"hello\nworld"
# => hello
# => world
```

Les caractères d'échappement vous permettent d'ajouter rapidement un caractère qui serait autrement difficile à taper.

Nushell supporte actuellement les caractères d'échappement suivants :

- `\"` - caractère de guillemet double
- `\'` - caractère de guillemet simple
