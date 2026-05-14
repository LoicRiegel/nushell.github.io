---
prev:
  text: Notes de conception
  link: /book/design_notes.md
next:
  text: (Pas si) Avancé
  link: /book/advanced.md
---

# Comment Nushell exécute du code

Dans [Penser en Nu](./thinking_in_nu.md#think-of-nushell-as-a-compiled-language), nous vous avons encouragé à _« Penser à Nushell comme un langage compilé »_ en raison de la manière dont le code Nushell est traité. Nous avons également couvert plusieurs exemples de code qui ne fonctionneront pas dans Nushell en raison de ce processus.

La raison sous-jacente de ceci est une séparation stricte des étapes **_analyse et évaluation_** qui **_interdit la fonctionnalité de type `eval`_**. Dans cette section, nous allons expliquer en détail ce que cela signifie, pourquoi nous le faisons, et quelles en sont les implications. L'explication vise à être aussi simple que possible, mais cela pourrait aider si vous avez programmé dans un autre langage auparavant.

[[toc]]

## Langages interprétés et compilés

### Langages interprétés

Nushell, Python et Bash (et bien d'autres) sont des langages _« interprétés »_.

Commençons par un simple programme Nushell « Hello, World! » :

```nu
# hello.nu

print "Hello, World!"
```

Bien sûr, cela s'exécute comme prévu en utilisant `nu hello.nu`. Un programme similaire écrit en Python ou Bash ressemblerait (et se comporterait) presque de la même manière.

Dans les _« langages interprétés »_, le code est généralement traité de cette façon :

```text
Code source → Interpréteur → Résultat
```

Nushell suit ce modèle, et son « Interpréteur » est divisé en deux parties :

1. `Code source → Analyseur → Représentation intermédiaire (IR)`
2. `IR → Moteur d'évaluation → Résultat`

Tout d'abord, le code source est analysé par l'Analyseur et converti en une représentation intermédiaire (IR), qui dans le cas de Nushell est juste une collection de structures de données. Ensuite, ces structures de données sont transmises au Moteur pour l'évaluation et la sortie des résultats.

C'est également courant dans les langages interprétés. Par exemple, le code source de Python est généralement [converti en bytecode](https://github.com/python/cpython/blob/main/InternalDocs/interpreter.md) avant l'évaluation.

### Langages compilés

De l'autre côté se trouvent les langages généralement « compilés », tels que C, C++ ou Rust. Par exemple, voici un simple _« Hello, World! »_ en Rust :

```rust
// main.rs

fn main() {
    println!("Hello, World!");
}
```

Pour « exécuter » ce code, il doit être :

1. Compilé en [instructions en code machine](https://en.wikipedia.org/wiki/Machine_code)
2. Les résultats de la compilation stockés sous forme de fichier binaire sur le disque

Les deux premières étapes sont traitées avec `rustc main.rs`.

3. Ensuite, pour produire un résultat, vous devez exécuter le binaire (`./main`), qui transmet les instructions à l'UC

Donc :

1. `Code source ⇒ Compilateur ⇒ Code machine`
2. `Code machine ⇒ UC ⇒ Résultat`

::: important
Vous pouvez voir que la séquence compile-run n'est pas très différente de la séquence parse-evaluate d'un interpréteur. Vous commencez par le code source, l'analysez (ou le compilez) dans un état (par ex., bytecode, IR, code machine), puis l'évaluez (ou l'exécutez) l'IR pour obtenir un résultat. Vous pourriez penser au code machine comme juste un autre type d'IR et à l'UC comme son interpréteur.

Une grande différence, cependant, entre les langages interprétés et compilés est que les langages interprétés implémentent généralement une fonction _`eval`_ tandis que les langages compilés ne le font pas. Qu'est-ce que cela signifie?
:::
