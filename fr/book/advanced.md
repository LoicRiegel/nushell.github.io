---
prev:
  text: Comment Nushell exécute du code
  link: /book/how_nushell_code_gets_run.md
next:
  text: Bibliothèque standard (Preview)
  link: /book/standard_library.md
---

# (Pas si) Avancé

Bien que le titre « Avancé » puisse sembler intimidant et que vous soyez tenté de sauter ce chapitre, en réalité, certaines des fonctionnalités les plus intéressantes et les plus puissantes s'y trouvent.

En plus des commandes intégrées, Nushell possède une [bibliothèque standard](standard_library.md).

Nushell opère sur des _données structurées_.
On pourrait dire que Nushell est un « shell orienté données » et un langage de programmation.
Pour explorer davantage la direction orientée données, Nushell inclut un moteur de traitement de données complet utilisant [Polars](https://github.com/pola-rs/polars) comme backend.
Assurez-vous de consulter la [documentation des DataFrames](dataframes.md) si vous souhaitez traiter efficacement de grandes quantités de données directement dans votre shell.

Les valeurs dans Nushell contiennent certaines [métadonnées](metadata.md) supplémentaires.
Ces métadonnées peuvent être utilisées, par exemple, pour [créer des erreurs personnalisées](creating_errors.md).

Grâce aux règles de scope strictes de Nushell, il est très facile de [itérer sur des collections en parallèle](parallelism.md), ce qui peut vous aider à accélérer les scripts longue durée en tapant juste quelques caractères.

Vous pouvez [explorer interactivement les données](explore.md) avec la commande [`explore`](/commands/docs/explore.md).

Enfin, vous pouvez étendre les fonctionnalités de Nushell avec des [plugins](plugins.md).
Presque n'importe quoi peut être un plugin tant qu'il communique avec Nushell selon un protocole que Nushell comprend.
