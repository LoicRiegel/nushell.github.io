# Travailler avec les enregistrements

:::tip
Les enregistrements sont à peu près équivalents aux lignes individuelles d'un tableau. Vous pouvez penser à un enregistrement comme étant essentiellement une « table à une ligne ». Ainsi, la plupart des commandes qui opèrent sur une ligne de tableau _fonctionnent également_ sur un enregistrement. Par exemple, [`update`](/commands/docs/update.md) peut être utilisé avec les enregistrements :

```nu
let my_record = {
 name: "Sam"
 age: 30
 }
$my_record | update age { $in + 1 }
# => ╭──────┬─────╮
# => │ name │ Sam │
# => │ age  │ 31  │
# => ╰──────┴─────╯
```

Notez que la [variable `my_record` est immuable](variables.md). L'enregistrement mis à jour résultant du [pipeline](pipelines.md) est imprimé comme vu dans le bloc de code. La variable `my_record` contient toujours la valeur originale - `$my_record.age` est toujours `30`.

:::

## Création d'enregistrements

Un enregistrement est une collection de zéro ou plus de mappages de paires clé-valeur. Il est similaire à un objet JSON et peut être créé en utilisant la même syntaxe :

```nu
# Nushell
{ "apples": 543, "bananas": 411, "oranges": 0 }
# => ╭─────────┬─────╮
# => │ apples  │ 543 │
# => │ bananas │ 411 │
# => │ oranges │ 0   │
# => ╰─────────┴─────╯
# JSON
'{ "apples": 543, "bananas": 411, "oranges": 0 }' | from json
# => ╭─────────┬─────╮
# => │ apples  │ 543 │
# => │ bananas │ 411 │
# => │ oranges │ 0   │
# => ╰─────────┴─────╯
```

Dans Nushell, les paires clé-valeur d'un enregistrement peuvent également être séparées en utilisant des espaces ou des sauts de ligne.

::: tip
Comme les enregistrements peuvent avoir de nombreux champs, ils sont, par défaut, affichés verticalement plutôt que de gauche à droite. Pour afficher un enregistrement de gauche à droite, convertissez-le en nuon. Par exemple :

```nu
  {
    name: "Sam"
```
