# Navigation et accès aux données structurées

Compte tenu du fort soutien de Nushell pour les données structurées, certaines des tâches les plus courantes impliquent la navigation et l'accès à ces données.

## Index de cette section

- [Arrière-plan et définitions](#background)
- [Chemins de cellule](#cell-paths)
  - [Avec des enregistrements](#records)
  - [Avec les listes](#lists)
  - [Avec les tableaux](#tables)
    - Données d'exemple
    - Exemple - Accès à une ligne de tableau
    - Exemple - Accès à une colonne de tableau
  - [Avec les données imbriquées](#nested-data)
- [Utilisation de `get` et `select`](#using-get-and-select)
  - Exemple - `get` vs. `select` avec une ligne de tableau
  - Exemple - `select` avec plusieurs lignes et colonnes
- [Gestion des données manquantes en utilisant l'opérateur optionnel](#the-optional-operator)
- [Noms de clé/colonne avec espaces](#keycolumn-names-with-spaces)
- [Autres commandes pour la navigation des données structurées](#other-commands-for-accessing-structured-data)

## Arrière-plan

Pour les exemples et descriptions ci-dessous, gardez à l'esprit plusieurs définitions concernant les données structurées :

- **_Liste :_** Les listes contiennent une série de zéro ou plus de valeurs de n'importe quel type. Une liste avec zéro valeurs est connue sous le nom de « liste vide »
- **_Enregistrement :_** Les enregistrements contiennent zéro ou plus de paires de clés nommées et leurs valeurs correspondantes. Les données dans la valeur d'un enregistrement peuvent également être de n'importe quel type. Un enregistrement avec zéro paires clé-valeur est connu sous le nom de « enregistrement vide »
- **_Données imbriquées :_** Les valeurs contenues dans une liste, un enregistrement ou un tableau peuvent être soit d'un type de base, soit des données structurées elles-mêmes. Cela signifie que les données peuvent être imbriquées sur plusieurs niveaux et sous plusieurs formes :
  - Les valeurs de liste peuvent contenir des tableaux, des enregistrements et même d'autres listes
    - **_Tableau :_** Les tableaux sont une liste d'enregistrements
  - Les valeurs d'enregistrement peuvent contenir des tableaux, des listes et d'autres enregistrements
    - Cela signifie que les enregistrements d'un tableau peuvent également contenir des tableaux imbriqués, des listes et d'autres enregistrements

::: tip
Parce qu'un tableau est une liste d'enregistrements, toute commande ou syntaxe qui fonctionne sur une liste fonctionnera également sur un tableau. L'inverse n'est pas nécessairement vrai; il existe certaines commandes et syntaxes qui fonctionnent sur les tableaux mais pas les listes.
:::

## Chemins de cellule

Un chemin de cellule est le moyen principal d'accéder aux valeurs dans les données structurées. Ce chemin est basé sur un concept similaire à celui d'une feuille de calcul, où les colonnes ont des noms et les lignes ont des nombres. Les noms et indices de chemins de cellule sont séparés par des points.

### Enregistrements

Pour un enregistrement, le chemin de cellule spécifie le nom d'une clé, qui est une `chaîne`.

#### Exemple - Accès à la valeur d'un enregistrement :

```nu
let my_record = {
    a: 5
    b: 42
  }
$my_record.b + 5
# => 47
```

### Listes

Pour une liste, le chemin de cellule spécifie la position (index) de la valeur dans la liste. C'est un `int`:

#### Exemple - Accès à la valeur d'une liste :

Rappelez-vous, les indices de liste sont basés sur 0.

```nu
let scoobies_list = [ Velma Fred Daphne Shaggy Scooby ]
$scoobies_list.2
# => Daphne
```

### Tableaux

- Pour accéder à une colonne, un chemin de cellule utilise le nom de la colonne, qui est une `chaîne`
- Pour accéder à une ligne, il utilise le numéro d'index de la ligne, qui est un `int`
- Pour accéder à une seule cellule, il utilise une combinaison du nom de colonne avec l'index de ligne.

Les prochains exemples utilisent le tableau suivant :

```nu

```
