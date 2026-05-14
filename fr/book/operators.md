# Opérateurs

Nushell supporte les opérateurs suivants pour les opérations mathématiques, logiques et de chaîne courantes :

| Opérateur          | Description                                                     |
| ------------------ | --------------------------------------------------------------- |
| `+`                | ajouter                                                         |
| `-`                | soustraire                                                      |
| `*`                | multiplier                                                      |
| `/`                | diviser                                                         |
| `//`               | division entière                                                |
| `mod`              | modulo                                                          |
| `**`               | exponentiation (puissance)                                      |
| `==`               | égal                                                            |
| `!=`               | pas égal                                                        |
| `<`                | inférieur à                                                     |
| `<=`               | inférieur ou égal                                               |
| `>`                | plus grand que                                                  |
| `>=`               | plus grand ou égal                                              |
| `=~` ou `like`     | correspondance regex / chaîne contient une autre               |
| `!~` ou `not-like` | correspondance regex inverse / chaîne *ne contient pas* une autre |
| `in`               | valeur dans la liste                                            |
| `not-in`           | valeur non dans la liste                                        |
| `has`              | la liste a la valeur                                            |
| `not-has`          | la liste n'a pas la valeur                                      |
| `not`              | logique non                                                     |
| `and`              | et deux expressions booléennes (court-circuits)                 |
| `or`               | ou deux expressions booléennes (court-circuits)                 |
| `xor`              | ou exclusif deux expressions booléennes                         |
| `bit-or`           | ou au niveau du bit                                             |
| `bit-xor`          | xor au niveau du bit                                            |
| `bit-and`          | et au niveau du bit                                             |
| `bit-shl`          | décalage au niveau du bit vers la gauche                        |
| `bit-shr`          | décalage au niveau du bit vers la droite                        |
| `starts-with`      | la chaîne commence par                                          |
| `ends-with`        | la chaîne se termine par                                        |
| `++`               | ajouter des listes                                              |


Les parenthèses peuvent être utilisées pour le regroupement pour spécifier l'ordre d'évaluation ou pour appeler des commandes et utiliser les résultats dans une expression.

## Ordre des opérations

Pour comprendre la préséance des opérations, vous pouvez exécuter la commande : `help operators | sort-by precedence -r`.

Présentées en ordre décroissant de préséance, l'article détaille les opérations comme suit :

- Parenthèses (`()`)
- Exponentiation/Puissance (`**`)
- Multiplier (`*`), Diviser (`/`), Division entière/floor (`//`) et Modulo (`mod`)
- Ajouter (`+`) et Soustraire (`-`)
- Décalage bit (`bit-shl`, `bit-shr`)
- Opérations de comparaison (`==`, `!=`, `<`, `>`, `<=`, `>=`), tests d'appartenance (`in`, `not-in`, `starts-with`, `ends-with`), correspondance regex (`=~`, `!~`) et ajout de liste (`++`)
- Et au niveau du bit (`bit-and`)
- Xor au niveau du bit (`bit-xor`)
- Ou au niveau du bit (`bit-or`)
- Et logique (`and`)
- Xor logique (`xor`)
- Ou logique (`or`)
- Opérations d'assignation
- Non logique (`not`)

```nu
3 * (1 + 2)
# => 9
```

## Types

Toutes les opérations n'ont pas de sens pour tous les types de données.
Si vous tentez d'effectuer une opération sur des types de données non compatibles, vous rencontrerez un message d'erreur qui devrait expliquer ce qui s'est passé :
```nu
"spam" - 1
# => Error: nu::parser::unsupported_operation (link)
# => 
# =>   × Types mismatched for operation.
# =>    ╭─[entry #49:1:1]
# =>  1 │ "spam" - 1
# =>    · ───┬── ┬ ┬
# =>    ·    │   │ ╰── int
