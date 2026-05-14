---
next:
  text: Programmation en Nu
  link: /book/programming_in_nu.md
---

# Variables spéciales

Nushell met à disposition et utilise un certain nombre de variables et constantes spéciales. Beaucoup d'entre elles sont mentionnées ou documentées ailleurs dans ce Livre, mais cette page
devrait inclure _toutes_ les variables pour référence.

[[toc]]

## `$nu`

La constante `$nu` est un enregistrement contenant plusieurs valeurs utiles :

- `default-config-dir` : Le répertoire où les fichiers de configuration sont stockés et lus.
- `config-path` : Le chemin du fichier de configuration principal Nushell, normalement `config.nu` dans le répertoire de configuration.
- `env-path` : Le fichier de configuration d'environnement optionnel, normalement `env.nu` dans le répertoire de configuration.
- `history-path` : Le fichier texte ou SQLite stockant l'historique des commandes.
- `loginshell-path` : Le fichier de configuration optionnel qui s'exécute pour les shells de connexion, normalement `login.nu` dans le répertoire de configuration.
- `plugin-path` : Le fichier du registre de plugins, normalement `plugin.msgpackz` dans le répertoire de configuration.
- `home-dir` : Le répertoire de base de l'utilisateur qui peut être accédé en utilisant le raccourci `~`.
- `data-dir` : Le répertoire des données pour Nushell, qui inclut les répertoires `./vendor/autoload` chargés au démarrage et d'autres données utilisateur.
- `cache-dir` : Un répertoire pour les données non essentielles (en cache).
- `vendor-autoload-dirs` : Une liste de répertoires où les applications tierces doivent installer les fichiers de configuration qui seront auto-chargés au démarrage.
- `user-autoload-dirs` : Une liste de répertoires où l'utilisateur peut créer des fichiers de configuration supplémentaires qui seront auto-chargés au démarrage.
- `temp-dir` : Un chemin pour les fichiers temporaires qui doit être inscriptible par l'utilisateur.
- `pid` : Le PID du processus Nushell actuellement exécuté.
- `os-info` : Informations sur le système d'exploitation hôte.
- `startup-time` : La durée (en durée) qu'il a fallu pour que Nushell démarre et traite tous les fichiers de configuration.
- `is-interactive` : Un booléen indiquant si Nushell a été démarré en tant que shell interactif (`true`) ou exécute un script ou une chaîne de commande. Par exemple :

  ```nu
  $nu.is-interactive
  # => true
  nu -c "$nu.is-interactive"
  # => false

  # Force interactive with --interactive (-i)
  nu -i -c "$nu.is-interactive"
  # => true
  ```

  Remarque : Lorsqu'il est démarré en tant que shell interactif, les fichiers de configuration de démarrage sont traités. Lorsqu'il est démarré en tant que shell non interactif, aucun fichier de configuration n'est lu sauf s'il est explicitement appelé via flag.

- `is-login` : Indique si Nushell a été démarré ou non en tant que shell de connexion.
- `history-enabled` : L'historique peut être désactivé via `nu --no-history`, auquel cas cette constante sera `false`.
- `current-exe` : Le chemin complet de l'exécutable `nu` actuellement exécuté. Peut être combiné avec `path dirname` (qui est constant) pour déterminer le répertoire où le binaire est situé.
