# Dataframes

::: warning Important!
Cette fonctionnalité nécessite le plugin Polars. Voir le
[chapitre Plugins](plugins.md) pour apprendre comment l'installer.

Pour tester que ce plugin est correctement installé, exécutez `help polars`.
:::

Comme nous l'avons vu jusqu'à présent, Nushell fait du travail avec les données sa priorité principale.
Les `Listes` et les `Tableaux` sont là pour vous aider à parcourir les valeurs afin de
effectuer plusieurs opérations ou trouver des données en un rien de temps. Cependant, il y a
certaines opérations où une disposition de données basée sur les lignes n'est pas le moyen le plus efficace
de traiter les données, en particulier lorsque vous travaillez avec des fichiers extrêmement volumineux. Les opérations
comme group-by ou join utilisant de grands ensembles de données peuvent être coûteuses en mémoire, et peuvent
conduire à de grands temps de calcul s'ils ne sont pas effectués en utilisant le format de données approprié.

Pour cette raison, la structure `DataFrame` a été introduite dans Nushell. Une
`DataFrame` stocke ses données dans un format en colonne en utilisant comme base la spécification [Apache
Arrow](https://arrow.apache.org/), et utilise
[Polars](https://github.com/pola-rs/polars) comme moteur pour effectuer
des [opérations en colonne extrêmement rapides](https://h2oai.github.io/db-benchmark/).

Vous vous demandez peut-être maintenant avec quelle rapidité ce combo pourrait fonctionner, et comment cela pourrait
faciliter le travail avec les données et le rendre plus fiable. Pour cette raison, nous commencerons ce
chapitre par présenter des benchmarks sur les opérations courantes qui sont effectuées lors du
traitement des données.

[[toc]]

## Comparaisons de benchmarks

Pour ce petit exercice de benchmark, nous comparerons
les commandes Nushell natives, les commandes Nushell de dataframe et les commandes [Python
Pandas](https://pandas.pydata.org/). Pour le moment, ne portez pas trop
d'attention aux commandes [`Dataframe`](/commands/categories/dataframe.md). Elles seront expliquées dans les sections ultérieures de cette page.

::: tip Détails du système
Les benchmarks présentés dans cette section ont été exécutés en utilisant un Macbook avec un processeur M1 pro et 32 Go de RAM. Tous les exemples ont été exécutés sur Nushell version 0.97 utilisant `nu_plugin_polars 0.97`.
:::

### Informations sur le fichier

Le fichier que nous utiliserons pour les benchmarks est le
ensemble de données [Démographie commerciale de la Nouvelle-Zélande](https://www.stats.govt.nz/assets/Uploads/New-Zealand-business-demography-statistics/New-Zealand-business-demography-statistics-At-February-2020/Download-data/Geographic-units-by-industry-and-statistical-area-2000-2020-descending-order-CSV.zip).
N'hésitez pas à le télécharger si vous souhaitez suivre ces tests.

L'ensemble de données a 5 colonnes et 5 429 252 lignes. Nous pouvons vérifier cela en utilisant le
