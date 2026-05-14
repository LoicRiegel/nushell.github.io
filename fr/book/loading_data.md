# Chargement des données

Précédemment, nous avons vu comment vous pouvez utiliser des commandes comme [`ls`](/commands/docs/ls.md), [`ps`](/commands/docs/ps.md), [`date`](/commands/docs/date.md) et [`sys`](/commands/docs/sys.md) pour charger des informations sur vos fichiers, processus, heure du jour et le système lui-même. Chaque commande nous donne un tableau d'informations que nous pouvons explorer. Il y a d'autres façons de charger un tableau de données avec lequel travailler.

## Ouverture de fichiers

L'un des atouts les plus puissants de Nu en matière de travail avec les données est la commande [`open`](/commands/docs/open.md). C'est un outil polyvalent qui peut travailler avec un certain nombre de formats de données différents. Pour voir ce que cela signifie, essayons d'ouvrir un fichier json :

@[code](@snippets/loading_data/vscode.sh)

De manière similaire à [`ls`](/commands/docs/ls.md), ouvrir un type de fichier que Nu comprend nous donnera quelque chose qui est plus que juste du texte (ou un flux d'octets). Ici, nous ouvrons un fichier « package.json » d'un projet JavaScript. Nu peut reconnaître le texte JSON et l'analyser en un tableau de données.

Si nous voulions vérifier la version du projet que nous regardions, nous pouvons utiliser la commande [`get`](/commands/docs/get.md).

```nu
open editors/vscode/package.json | get version
# => 1.0.0
```

Nu supporte actuellement les formats suivants pour charger les données directement dans les tableaux :

- csv
- eml
- ics
- ini
- json
- [nuon](#nuon)
- ods
- [Bases de données SQLite](#sqlite)
- ssv
- toml
- tsv
- url
- vcf
- xlsx / xls
- xml
- yaml / yml

::: tip Le saviez-vous?
Sous le capot, `open` cherchera une sous-commande `from ...` dans votre scope qui correspond à l'extension de votre fichier.
Vous pouvez ainsi simplement étendre l'ensemble des types de fichiers supportés de `open` en créant votre propre sous-commande `from ...`.
:::

Mais qu'arrive-t-il si vous chargez un fichier texte qui n'est pas un de ceux-ci ? Essayons :

```nu
open README.md
```

Nous voyons le contenu du fichier.

Sous la surface, ce que Nu voit dans ces fichiers texte est une grande chaîne. Ensuite, nous allons parler de la façon de travailler avec ces chaînes pour obtenir les données dont nous avons besoin.

## NUON

Nushell Object Notation (NUON) vise à être pour Nushell ce que JavaScript Object Notation (JSON) est pour JavaScript.
C'est-à-dire que le code NUON est un code Nushell valide qui décrit une structure de données.
Par exemple, ceci est un NUON valide (exemple du [fichier de configuration par défaut](https://github.com/nushell/nushell/blob/main/crates/nu-utils/src/default_files/default_config.nu)) :

```nu
{
  menus: [
    # Configuration for default nushell menus
    # Note the lack of source parameter
    {
      name: completion_menu
      only_buffer_difference: false
      marker: "| "
      type: {
          layout: columnar
          columns: 4
          col_width: 20   # Optional value. If missing all the screen width is used to calculate column width
          col_padding: 2
      }
      style: {
          text: green
          selected_text: green_reverse
          description_text: yellow
      }
    }
```
