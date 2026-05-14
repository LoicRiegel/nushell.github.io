---
prev:
  text: Meilleures pratiques
  link: /book/style_guide.md
next:
  text: Configuration
  link: /book/configuration.md
---

# Nu comme un shell

Le chapitre [Fondamentaux de Nushell](nu_fundamentals.md) et [Programmation en Nu](programming_in_nu.md) se sont concentrés principalement sur les aspects du langage de Nushell.
Ce chapitre met en lumière les parties de Nushell qui sont liées à l'interpréteur Nushell (la Nushell [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)).
Certains des concepts font directement partie du langage de programmation Nushell (comme les variables d'environnement) tandis que d'autres sont implémentés purement pour améliorer l'expérience interactive (comme les hooks) et ne sont donc pas présents, par exemple, lors de l'exécution d'un script.

De nombreux paramètres de Nushell peuvent être [configurés](configuration.md).
La configuration elle-même est stockée comme une variable d'environnement.
De plus, Nushell a plusieurs fichiers de configuration différents qui s'exécutent au démarrage où vous pouvez mettre des commandes personnalisées, des alias, etc.

Une grande caractéristique de tout shell est celle des [variables d'environnement](environment.md).
Dans Nushell, les variables d'environnement sont scoped et peuvent avoir n'importe quel type supporté par Nushell.
Cela apporte certaines considérations de conception supplémentaires, veuillez donc consulter la section liée pour plus de détails.

Les autres sections expliquent comment travailler avec [stdout, stderr et codes de sortie](stdout_stderr_exit_codes.md), comment [exécuter une commande externe quand il y a un intégré avec le même nom](./running_externals.md), et comment [configurer des invites tiers](3rdpartyprompts.md) pour travailler avec Nushell.

Une fonctionnalité intéressante de Nushell est la [Pile de répertoires](directory_stack.md) qui vous permet de travailler dans plusieurs répertoires simultanément.

Nushell a également son propre éditeur de ligne [Reedline](line_editor.md).
Avec la configuration de Nushell, il est possible de configurer certaines des fonctionnalités de Reedline, telles que l'invite, les liaisons clavier, l'historique ou les menus.

Il est également possible de définir [des signatures personnalisées pour les commandes externes](externs.md) ce qui vous permet de définir [des complétions personnalisées](custom_completions.md) pour elles (les complétions personnalisées fonctionnent également pour les commandes personnalisées de Nushell).

[Coloration et thème dans Nushell](coloring_and_theming.md) entre dans plus de détails sur comment configurer l'apparence de Nushell.

Si vous souhaitez planifier certaines commandes à exécuter en arrière-plan, [Tâches de fond](background_jobs.md) fournit des directives simples à suivre.

Enfin, [les hooks](hooks.md) vous permettent d'insérer des fragments de code Nushell à exécuter lors de certains événements.
