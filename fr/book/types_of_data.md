---
prev:
  text: Fondamentaux de Nushell
  link: /book/nu_fundamentals.md
---

# Types de données

Les commandes shell Unix traditionnels communiquent entre elles en utilisant des chaînes de texte -- Une commande écrit du texte sur la sortie standard (souvent abrégée `stdout`) et l'autre lit le texte de l'entrée standard (ou `stdin`). Cela permet de combiner plusieurs commandes ensemble pour communiquer via ce qu'on appelle un « pipeline ».

Nushell adopte cette approche et l'étend pour inclure d'autres types de données en plus des chaînes.

Comme de nombreux langages de programmation, Nu modélise les données en utilisant un ensemble de types de données simples et structurés. Les types de données simples incluent les entiers, les flottants, les chaînes et les booléens. Il existe également des types spéciaux pour les dates, les tailles de fichiers et les durées.

La commande [`describe`](/commands/docs/describe.md) retourne le type d'une valeur de données :

```nu
42 | describe
# => int
```

## Types en un coup d'œil

| Type                                          | Exemple                                                               |
| --------------------------------------------- | --------------------------------------------------------------------- |
| [Entiers](#entiers)                           | `-65535`                                                              |
| [Flottants (décimales)](#flottants-décimales) | `9.9999`, `Infinity`                                                  |
| [Chaînes](#text-strings)                      | <code>"hole 18", 'hole 18', \`hole 18\`, hole18, r#'hole18'#</code>   |
| [Booléens](#booléens)                         | `true`                                                                |
| [Dates](#dates)                               | `2000-01-01`                                                          |
| [Durées](#durées)                             | `2min + 12sec`                                                        |
| [Tailles de fichiers](#tailles-de-fichiers)   | `64mb`                                                                |
| [Plages](#plages)                             | `0..4`, `0..<5`, `0..`, `..4`                                         |
| [Binaire](#données-binaires)                  | `0x[FE FF]`                                                           |
| [Listes](#listes)                             | `[0 1 'two' 3]`                                                       |
| [Enregistrements](#enregistrements)           | `{name:"Nushell", lang: "Rust"}`                                      |
| [Tableaux](#tableaux)                         | `[{x:12, y:15}, {x:8, y:9}]`, `[[x, y]; [12, 15], [8, 9]]`            |
| [Fermetures](#fermetures)                     | `{\|e\| $e + 1 \| into string }`, `{ $in.name.0 \| path exists }`     |
| [Chemins de cellule](#chemins-de-cellule)     | `$.name.0`                                                            |
| [Blocs](#blocs)                               | `if true { print "hello!" }`, `loop { print "press ctrl-c to exit" }` |
| [Null (Rien)](#nothing-null)                  | `null`                                                                |
| [Quelconque](#quelconque)                     | `let p: any = 5`                                                      |

## Types de données de base

### Entiers

|                     |                                                              |
| ------------------- | ------------------------------------------------------------ |
| **_Description :_** | Nombres sans composant fractionnaire (positif, négatif et 0) |
| **_Annotation :_**  | `int`                                                        |
