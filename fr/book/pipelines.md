# Pipelines

L'une des conceptions centrales de Nu est le pipeline, une idée de conception qui remonte à ses racines il y a des décennies à une philosophie originale d'Unix. Tout comme Nu s'étend du type de données de chaîne unique d'Unix, Nu étend également l'idée du pipeline pour inclure plus que du simple texte.

## Bases

Un pipeline est composé de trois parties : l'entrée, le filtre et la sortie.

```nu
open Cargo.toml | update workspace.dependencies.base64 0.24.2 | save Cargo_new.toml
```

La première commande, `open Cargo.toml`, est une entrée (parfois aussi appelée une « source » ou un « producteur »). Cela crée ou charge les données et les alimente dans un pipeline. C'est de l'entrée que les pipelines obtiennent les valeurs avec lesquelles travailler. Les commandes comme [`ls`](/commands/docs/ls.md) sont également des entrées, car elles prennent les données du système de fichiers et les envoient par les pipelines afin qu'elles puissent être utilisées.

La deuxième commande, `update workspace.dependencies.base64 0.24.2`, est un filtre. Les filtres prennent les données qui leur sont données et font souvent quelque chose avec. Ils peuvent les modifier (comme avec la commande [`update`](/commands/docs/update.md) dans notre exemple), ou ils peuvent effectuer d'autres opérations, comme la journalisation, au fur et à mesure que les valeurs traversent.

La dernière commande, `save Cargo_new.toml`, est une sortie (parfois appelée un « sink »). Une sortie prend l'entrée du pipeline et fait une dernière opération sur elle. Dans notre exemple, nous enregistrons ce qui traverse le pipeline vers un fichier comme dernière étape. D'autres types de commandes de sortie peuvent prendre les valeurs et les afficher pour l'utilisateur.

La variable `$in` collectera le pipeline en une valeur pour vous, vous permettant d'accéder au flux entier comme paramètre :

```nu
[1 2 3] | $in.1 * $in.2
# => 6
```

## Pipelines multi-lignes

Si un pipeline devient un peu long pour une ligne, vous pouvez le mettre entre parenthèses `()` :

```nu
let year = (
    "01/22/2021" |
    parse "{month}/{day}/{year}" |
    get year
)
```

## Points-virgules

Prenez cet exemple :

```nu
line1; line2 | line3
```

Ici, les points-virgules sont utilisés en conjonction avec les pipelines. Quand un point-virgule est utilisé, aucune donnée de sortie n'est produite pour être piped. À cet égard, la variable `$in` ne fonctionnera pas quand elle est utilisée immédiatement après le point-virgule.

- Comme il y a un point-virgule après `line1`, la commande s'exécutera jusqu'à la fin et sa sortie sera rejetée.
- `line2` | `line3` est un pipeline normal. Il s'exécute, et comme la valeur finale, son contenu est retourné et affiché.
