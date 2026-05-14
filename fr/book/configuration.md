---
prev:
  text: Nu comme shell
  link: /book/nu_as_a_shell.md
---

# Configuration

## Démarrage rapide

Bien que Nushell fournisse de nombreuses options pour gérer son démarrage et sa configuration, les nouveaux utilisateurs
peuvent commencer avec juste quelques étapes simples :

1. Dites à Nushell quel éditeur utiliser :

   ```nu
   $env.config.buffer_editor = <path_to_your_preferred_editor>
   ```

   Par exemple :

   ```nu
   $env.config.buffer_editor = ["code", "-w"]
   # or
   $env.config.buffer_editor = "nano"
   # or
   $env.config.buffer_editor = "hx"
   # or
   $env.config.buffer_editor = "vi"
   # with args
   $env.config.buffer_editor = ["emacsclient", "-s", "light", "-t"]
   # etc.
   ```

2. Modifiez `config.nu` en utilisant :

   ```nu
   config nu
   ```

   Cela ouvrira le `config.nu` actuel dans l'éditeur défini ci-dessus.

3. Ajoutez des commandes à ce fichier qui doivent s'exécuter chaque fois que Nushell démarre. Un bon premier exemple pourrait être le paramètre `buffer_editor` ci-dessus.

   Vous pouvez trouver une liste détaillée des paramètres disponibles en utilisant :

   ```nu
    config nu --doc | nu-highlight | less -R
   ```
