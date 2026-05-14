# Coloration et thème dans Nu

De nombreuses parties de l'interface de Nushell peuvent avoir leurs couleurs personnalisées. Tous ceux-ci peuvent être définis dans le fichier de configuration `config.nu`. Si vous voyez le `#` en dehors d'une valeur de texte dans le fichier de configuration, cela signifie que le texte après celui-ci est commenté.

## Bordures de tableau

Les bordures du tableau sont contrôlées par le paramètre `$env.config.table.mode`. Il peut être modifié à l'exécution ou dans le fichier `config.nu` :

```nu
$env.config.table.mode = 'rounded'
```

Les options pour `$env.config.table.mode` peuvent être listées avec `table --list` :

<!-- Généré avec table --list | each {|| $"- `($in)`"} | sort | str join "\n"` -->

- `ascii_rounded`
- `basic_compact`
- `basic`
- `compact_double`
- `compact`
- `default`
- `dots`
- `double`
- `heavy`
- `light`
- `markdown`
- `none`
- `psql`
- `reinforced`
- `restructured`
- `rounded`
- `single`
- `thin`
- `with_love`

Exemples :

```nu
$env.config.table.mode = 'rounded'
table --list | first 5
# => ╭───┬────────────────╮
# => │ 0 │ basic          │
# => │ 1 │ compact        │
# => │ 2 │ compact_double │
# => │ 3 │ default        │
# => │ 4 │ heavy          │
# => ╰───┴────────────────╯

$env.config.table.mode = 'psql'
table --list | first 5
# =>  0 | basic
# =>  1 | compact
# =>  2 | compact_double
# =>  3 | default
# =>  4 | heavy
```

## Configuration de la couleur

La configuration de la couleur est définie dans `$env.config.color_config`. La configuration actuelle peut être imprimée avec :

```nu
$env.config.color_config | sort
```

La couleur et les attributs de style peuvent être déclarés dans plusieurs formats alternatifs.

- `r` - abréviation de la couleur normale rouge
- `rb` - abréviation de la couleur normale rouge avec attribut gras
- `red` - couleur rouge normale
- `red_bold` - couleur rouge normale avec attribut gras
- `"#ff0000"` - couleur rouge au format "#hex" de premier plan (des guillemets sont requis)
- `{ fg: "#ff0000" bg: "#0000ff" attr: b }` - format complet "#hex" avec rouge au format "#hex" au premier plan avec un arrière-plan de bleu au format "#hex" avec un attribut de gras abrégé.
- `{|x| 'yellow' }` - fermeture renvoyant une chaîne avec l'une des représentations de couleur listées ci-dessus
- `{|x| { fg: "#ff0000" bg: "#0000ff" attr: b } }` - fermeture renvoyant un enregistrement valide

### Attributs

| code | sens          |
| ---- | ------------- |
| l    | clignoter     |
| b    | gras          |
| d    | atténué       |
| h    | caché         |
| i    | italique      |
| r    | inverse       |
| s    | barré         |
| u    | souligné      |
| n    | rien          |
|      | défaut à rien |

### Couleurs normales et abréviations

| code    | nom                     |
| ------- | ----------------------- |
| `g`     | `green`                 |
| `gb`    | `green_bold`            |
| `gu`    | `green_underline`       |
| `gi`    | `green_italic`          |
| `gd`    | `green_dimmed`          |
| `gr`    | `green_reverse`         |
| `bg_g`  | `bg_green`              |
| `lg`    | `light_green`           |
| `lgb`   | `light_green_bold`      |
| `lgu`   | `light_green_underline` |
| `lgi`   | `light_green_italic`    |
| `lgd`   | `light_green_dimmed`    |
| `lgr`   | `light_green_reverse`   |
| `bg_lg` | `bg_light_green`        |
| `r`     | `red`                   |
| `rb`    | `red_bold`              |
| `ru`    | `red_underline`         |
| `ri`    | `red_italic`            |
| `rd`    | `red_dimmed`            |
| `rr`    | `red_reverse`           |
| `bg_r`  | `bg_red`                |
| `lr`    | `light_red`             |
| `lrb`   | `light_red_bold`        |
| `lru`   | `light_red_underline`   |
| `lri`   | `light_red_italic`      |
| `lrd`   | `light_red_dimmed`      |
| `lrr`   | `light_red_reverse`     |
| `bg_lr` | `bg_light_red`          |
| `u`     | `blue`                  |
| `ub`    | `blue_bold`             |
| `uu`    | `blue_underline`        |
| `ui`    | `blue_italic`           |
| `ud`    | `blue_dimmed`           |
| `ur`    | `blue_reverse`          |
| `bg_u`  | `bg_blue`               |
| `lu`    | `light_blue`            |
| `lub`   | `light_blue_bold`       |
| `luu`   | `light_blue_underline`  |
| `lui`   | `light_blue_italic`     |
| `lud`   | `light_blue_dimmed`     |
| `lur`   | `light_blue_reverse`    |
| `bg_lu` | `bg_light_blue`         |

### Format `"#hex"`

Le format "#hex" est une façon typique de représenter les couleurs. C'est simplement le caractère `#` suivi de 6 caractères. Les deux premiers sont pour `red`, les deux suivants pour `green` et les deux derniers pour `blue`. Il est important que cette chaîne soit entourée de guillemets, sinon Nushell pense que c'est une chaîne commentée.

Exemple : La couleur `red` primaire est `"#ff0000"` ou `"#FF0000"`. Les majuscules et minuscules dans les lettres ne devraient pas faire de différence.

Ce format `"#hex"` nous permet de spécifier des teintes en truecolor 24 bits à différentes parties de Nushell.

### Format complet `"#hex"`

Le format complet `"#hex"` s'inspire du format `"#hex"` mais permet de spécifier le premier plan, l'arrière-plan et les attributs sur une seule ligne.

Exemple : `{ fg: "#ff0000" bg: "#0000ff" attr: b }`

- premier plan rouge au format "#hex"
- arrière-plan bleu au format "#hex"
- attribut de gras abrégé

### Fermeture

Remarque : Les fermetures ne sont exécutées que pour la sortie du tableau. Elles ne fonctionnent pas dans d'autres contextes comme pour les configurations `shape_`, lors de l'impression d'une valeur directement ou comme une valeur dans une liste.

Par exemple :

```nu
$env.config.color_config.filesize = {|x| if $x == 0b { 'dark_gray' } else if $x < 1mb { 'cyan' } else { 'blue' } }
$env.config.color_config.bool = {|x| if $x { 'green' } else { 'light_red' } }
{a:true,b:false,c:0mb,d:0.5mb,e:10mib}
```

imprime

```nu
╭───┬───────────╮
│ a │ true      │
│ b │ false     │
│ c │ 0 B       │
│ d │ 488.3 KiB │
│ e │ 10.0 MiB  │
╰───┴───────────╯
```

avec un `true` vert, un `false` rouge clair, un `0 B` gris foncé, un `488.3 KiB` cyan et un `10.0 MiB` bleu.

## Valeurs primitives

Les valeurs primitives sont des choses comme `int` et `string`. Les valeurs primitives et les formes peuvent être définies avec une variété de symbologies de couleur vues ci-dessus.

Ceci est la liste actuelle des primitives. Tous ces éléments ne sont pas configurables. Les configurable sont marqués avec \*.

| primitive    | couleur par défaut    | configurable |
| ------------ | --------------------- | ------------ |
| `any`        |                       |              |
| `binary`     | Color::White.normal() | \*           |
| `block`      | Color::White.normal() | \*           |
| `bool`       | Color::White.normal() | \*           |
| `cell-path`  | Color::White.normal() | \*           |
| `condition`  |                       |              |
| `custom`     |                       |              |
| `datetime`   | Color::White.normal() | \*           |
| `duration`   | Color::White.normal() | \*           |
| `expression` |                       |              |
| `filesize`   | Color::White.normal() | \*           |
| `float`      | Color::White.normal() | \*           |
| `glob`       |                       |              |
| `import`     |                       |              |
| `int`        | Color::White.normal() | \*           |
| `list`       | Color::White.normal() | \*           |
| `nothing`    | Color::White.normal() | \*           |
| `number`     |                       |              |
| `operator`   |                       |              |
| `path`       |                       |              |
| `range`      | Color::White.normal() | \*           |
| `record`     | Color::White.normal() | \*           |
| `signature`  |                       |              |
| `string`     | Color::White.normal() | \*           |
| `table`      |                       |              |
| `var`        |                       |              |
| `vardecl`    |                       |              |
| `variable`   |                       |              |

### Primitives spéciaux (pas vraiment des primitives mais ils existent uniquement pour la coloration)

| primitive                   | couleur par défaut         | configurable |
| --------------------------- | -------------------------- | ------------ |
| `leading_trailing_space_bg` | Color::Rgb(128, 128, 128)) | \*           |
| `header`                    | Color::Green.bold()        | \*           |
| `empty`                     | Color::Blue.normal()       | \*           |
| `row_index`                 | Color::Green.bold()        | \*           |
| `hints`                     | Color::DarkGray.normal()   | \*           |

Voici un petit exemple de modification de certaines de ces valeurs.

```nu
$env.config.color_config.separator = purple
$env.config.color_config.leading_trailing_space_bg = "#ffffff"
$env.config.color_config.header = gb
$env.config.color_config.datetime = wd
$env.config.color_config.filesize = c
$env.config.color_config.row_index = cb
$env.config.color_config.bool = red
$env.config.color_config.int = green
$env.config.color_config.duration = blue_bold
$env.config.color_config.range = purple
$env.config.color_config.float = red
$env.config.color_config.string = white
$env.config.color_config.nothing = red
$env.config.color_config.binary = red
$env.config.color_config.cell-path = cyan
$env.config.color_config.hints = dark_gray
```

Voici un autre petit exemple utilisant plusieurs syntaxes de couleur avec certains commentaires.

```nu
$env.config.color_config.separator = "#88b719" # cela définit uniquement la couleur de premier plan comme PR #486
$env.config.color_config.leading_trailing_space_bg = white # cela définit uniquement la couleur de premier plan dans le style d'origine
$env.config.color_config.header = { # c'est comme PR #489
    fg: "#B01455", # remarque, les guillemets sont requis sur les valeurs avec couleurs hex
    bg: "#ffb900", # remarque, les virgules ne sont pas obligatoires, cela pourrait aussi être sur une seule ligne
    attr: bli # remarque, il n'y a pas de guillemets autour de cette valeur. cela fonctionne avec ou sans guillemets
}
$env.config.color_config.datetime = "#75507B"
$env.config.color_config.filesize = "#729fcf"
$env.config.color_config.row_index = {
    # remarque, que c'est une autre façon de définir uniquement le premier plan, pas besoin de spécifier bg et attr
    fg: "#e50914"
}
```

## Valeurs de forme

Comme mentionné ci-dessus, `shape` est un terme utilisé pour indiquer la coloration de la syntaxe.

Voici la liste actuelle des formes plates.

| shape                        | style par défaut                       | configurable |
| ---------------------------- | -------------------------------------- | ------------ |
| `shape_block`                | fg(Color::Blue).bold()                 | \*           |
| `shape_bool`                 | fg(Color::LightCyan)                   | \*           |
| `shape_custom`               | bold()                                 | \*           |
| `shape_external`             | fg(Color::Cyan)                        | \*           |
| `shape_externalarg`          | fg(Color::Green).bold()                | \*           |
| `shape_filepath`             | fg(Color::Cyan)                        | \*           |
| `shape_flag`                 | fg(Color::Blue).bold()                 | \*           |
| `shape_float`                | fg(Color::Purple).bold()               | \*           |
| `shape_garbage`              | fg(Color::White).on(Color::Red).bold() | \*           |
| `shape_globpattern`          | fg(Color::Cyan).bold()                 | \*           |
| `shape_int`                  | fg(Color::Purple).bold()               | \*           |
| `shape_internalcall`         | fg(Color::Cyan).bold()                 | \*           |
| `shape_list`                 | fg(Color::Cyan).bold()                 | \*           |
| `shape_literal`              | fg(Color::Blue)                        | \*           |
| `shape_nothing`              | fg(Color::LightCyan)                   | \*           |
| `shape_operator`             | fg(Color::Yellow)                      | \*           |
| `shape_pipe`                 | fg(Color::Purple).bold()               | \*           |
| `shape_range`                | fg(Color::Yellow).bold()               | \*           |
| `shape_record`               | fg(Color::Cyan).bold()                 | \*           |
| `shape_signature`            | fg(Color::Green).bold()                | \*           |
| `shape_string`               | fg(Color::Green)                       | \*           |
| `shape_string_interpolation` | fg(Color::Cyan).bold()                 | \*           |
| `shape_table`                | fg(Color::Blue).bold()                 | \*           |
| `shape_variable`             | fg(Color::Purple)                      | \*           |

Voici un petit exemple de comment appliquer la couleur à ces éléments. Tout ce qui ne sera pas remplacé recevra sa couleur par défaut.

```nu
$env.config.color_config.shape_garbage: { fg: "#FFFFFF" bg: "#FF0000" attr: b}
$env.config.color_config.shape_bool: green
```
