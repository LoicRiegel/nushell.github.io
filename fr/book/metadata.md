# Métadonnées

En utilisant Nu, vous avez peut-être rencontré des moments où vous aviez l'impression que quelque chose d'extra se passait en arrière-plan. Par exemple, disons que vous essayez d'ouvrir un fichier que Nu supporte seulement pour oublier et essayer de convertir à nouveau :

```nu
open Cargo.toml | from toml
# => error: Expected a string from pipeline
# => - shell:1:18
# => 1 | open Cargo.toml | from toml
# =>   |                   ^^^^^^^^^ requires string input
# => - shell:1:5
# => 1 | open Cargo.toml | from toml
# =>   |      ---------- object originates from here
```

Le message d'erreur nous indique non seulement que ce que nous avons donné à [`from toml`](/commands/docs/from_toml.md) n'était pas une chaîne, mais aussi où la valeur venait à l'origine. Comment saurait-il cela?

Les valeurs qui transitent par un pipeline dans Nu ont souvent un ensemble d'informations supplémentaires, ou métadonnées, attachées à elles. Celles-ci sont connues sous le nom de tags, comme les tags sur un article dans un magasin. Ces tags n'affectent pas les données, mais ils donnent à Nu un moyen d'améliorer l'expérience de travail avec ces données.

Exécutons la commande [`open`](/commands/docs/open.md) à nouveau, mais cette fois, nous examinerons les tags qu'elle retourne :

```nu
metadata (open Cargo.toml)
# => ╭──────┬───────────────────╮
# => │ span │ {record 2 fields} │
# => ╰──────┴───────────────────╯
```

Actuellement, nous ne suivons que la portée d'où proviennent les valeurs. Regardons cela de plus près :

```nu
metadata (open Cargo.toml) | get span
# => ╭───────┬────────╮
# => │ start │ 212970 │
# => │ end   │ 212987 │
# => ╰───────┴────────╯
```

La portée « start » et « end » ici se réfèrent à l'endroit où le soulignage se trouvera dans la ligne. Si vous comptez plus de 5, puis comptez jusqu'à 15, vous verrez qu'il s'aligne avec le nom de fichier « Cargo.toml ». C'est ainsi que l'erreur que nous avons vue plus tôt savait quoi souligner.

## Métadonnées personnalisées

Vous pouvez joindre des métadonnées arbitraires aux données du pipeline en utilisant la commande [`metadata set`](/commands/docs/metadata_set.md) avec le paramètre de fermeture optionnel :

```nu
"data" | metadata set { merge {custom_key: "custom_value"} }
```

## Métadonnées de réponse HTTP

Toutes les commandes HTTP attachent les métadonnées de réponse :

```nu
http get https://api.example.com | metadata | get http_response.status
# => 200
```

Pour travailler avec les métadonnées lors de la diffusion en continu des corps de réponse, consultez le [cookbook HTTP](/cookbook/http.html#accessing-http-response-metadata-while-streaming).
