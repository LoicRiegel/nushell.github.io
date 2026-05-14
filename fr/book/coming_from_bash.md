---
prev:
  text: Venir à Nu
  link: /book/coming_to_nu.md
---
# Venir de Bash

::: tip
Si vous venez de `Git Bash` sur Windows, alors les commandes externes auxquelles vous êtes habitué (par ex, `ln`, `grep`, `vi`, etc) ne seront pas disponibles dans Nushell par défaut à moins que vous ne les ayez déjà explicitement rendues disponibles dans la variable d'environnement du chemin Windows.
Pour rendre ces commandes disponibles dans Nushell également, ajoutez la ligne suivante à votre `config.nu` avec soit `append` soit `prepend`.

```nu
$env.Path = ($env.Path | prepend 'C:\Program Files\Git\usr\bin')
```
:::

## Équivalents de commandes :

| Bash                                 | Nu                                                            | Tâche                                                              |
| ------------------------------------ | ------------------------------------------------------------- | ----------------------------------------------------------------- |
| `ls`                                 | `ls`                                                          | Liste les fichiers du répertoire courant                          |
| `ls <dir>`                           | `ls <dir>`                                                    | Liste les fichiers du répertoire donné                            |
| `ls pattern*`                        | `ls pattern*`                                                 | Liste les fichiers correspondant à un motif donné                 |
| `ls -la`                             | `ls --long --all` ou `ls -la`                                 | Lister les fichiers avec toutes les informations disponibles, y compris les fichiers cachés |
| `ls -d */`                           | `ls \| where type == dir`                                     | Lister les répertoires                                            |
| `find . -name *.rs`                  | `ls **/*.rs`                                                  | Trouver récursivement tous les fichiers correspondant à un motif donné |
| `find . -name Makefile \| xargs vim` | `ls **/Makefile \| get name \| vim ...$in`                    | Passer les valeurs comme paramètres de commande                  |
| `cd <directory>`                     | `cd <directory>`                                              | Changer vers le répertoire donné                                 |
| `cd`                                 | `cd`                                                          | Changer vers le répertoire personnel                             |
| `cd -`                               | `cd -`                                                        | Changer vers le répertoire précédent                             |
| `mkdir <path>`                       | `mkdir <path>`                                                | Crée le chemin donné                                              |
| `mkdir -p <path>`                    | `mkdir <path>`                                                | Crée le chemin donné, créant les parents si nécessaire            |
| `touch test.txt`                     | `touch test.txt`                                              | Créer un fichier                                                  |
| `> <path>`                           | `out> <path>` ou `o> <path>`                                  | Enregistrer la sortie de la commande dans un fichier              |
|                                      | `\| save <path>`                                              | Enregistrer la sortie de la commande dans un fichier en tant que données structurées |
| `>> <path>`                          | `out>> <path>` ou `o>> <path>`                                | Ajouter la sortie de la commande à un fichier                    |
|                                      | `\| save --append <path>`                                     | Ajouter la sortie structurée à un fichier                        |
| `> /dev/null`                        | `\| ignore`                                                   | Ignorer la sortie de la commande                                  |
| `> /dev/null 2>&1`                   | `out+err>\| ignore` ou `o+e>\| ignore`                        | Ignorer la sortie de la commande, y compris stderr                |
| `command 2>&1 \| less`               | `command out+err>\| less` ou `command o+e>\| less`            | Tuyauter stdout et stderr d'une commande externe dans less (REMARQUE : utiliser [explore](explore.html) pour paginer la sortie des commandes internes) |
| `cmd1 \| tee log.txt \| cmd2`        | `cmd1 \| tee { save log.txt } \| cmd2`                        | Tee la sortie de la commande dans un fichier journal             |
| `command \| head -5`                 | `command \| first 5`                                          | Limiter la sortie aux 5 premières lignes d'une commande interne  |
| `cat <path>`                         | `open --raw <path>`                                           | Afficher le contenu du fichier donné                              |
|                                      | `open <path>`                                                 | Lire un fichier en tant que données structurées                   |
| `cat <(<command1>) <(<command2>)`    | `[(command1), (command2)] \| str join`                        | Concaténer les sorties de command1 et command2                   |
| `cat <path> <(<command>)`            | `[(open --raw <path>), (command)] \| str join`                | Concaténer le contenu du fichier donné et la sortie de commande  |
| `mv <source> <dest>`                 | `mv <source> <dest>`                                          | Déplacer le fichier vers un nouvel emplacement                   |
| `for f in *.md; do echo $f; done`    | `ls *.md \| each { $in.name }`                                | Itérer sur une liste et retourner les résultats                  |
| `for i in $(seq 1 10); do echo $i; done` | `for i in 1..10 { print $i }`                             | Itérer sur une liste et exécuter une commande sur les résultats |
| `cp <source> <dest>`                 | `cp <source> <dest>`                                          | Copier le fichier vers un nouvel emplacement                     |
| `cp -r <source> <dest>`              | `cp -r <source> <dest>`                                       | Copier le répertoire vers un nouvel emplacement, récursivement   |
| `rm <path>`                          | `rm <path>`                                                   | Supprimer le fichier donné                                        |
|                                      | `rm -t <path>`                                                | Déplacer le fichier donné vers la corbeille système              |
| `rm -rf <path>`                      | `rm -r <path>`                                                | Supprime récursivement le chemin donné                            |
| `date -d <date>`                     | `"<date>" \| into datetime -f <format>`                       | Analyser une date ([documentation de format](https://docs.rs/chrono/0.4.15/chrono/format/strftime/index.html)) |
| `sed`                                | `str replace`                                                 | Trouver et remplacer un motif dans une chaîne                    |
| `grep <pattern>`                     | `where $it =~ <substring>` ou `find <substring>`              | Filtrer les chaînes contenant la sous-chaîne                     |
| `man <command>`                      | `help <command>`                                              | Obtenir l'aide pour une commande                                 |
|                                      | `help commands`                                               | Lister toutes les commandes disponibles                          |
|                                      | `help --find <string>`                                        | Rechercher une correspondance dans toutes les commandes disponibles |
| `command1 && command2`               | `command1; command2`                                          | Exécuter une commande, et si elle réussit, exécuter une seconde  |
| `stat $(which git)`                  | `stat ...(which git).path`                                    | Utiliser la sortie de la commande comme argument pour une autre commande |
| `echo /tmp/$RANDOM`                  | `$"/tmp/(random int)"`                                        | Utiliser la sortie de la commande dans une chaîne               |
| `cargo b --jobs=$(nproc)`            | `cargo b $"--jobs=(sys cpu \| length)"`                       | Utiliser la sortie de la commande dans une option                |
| `echo $PATH`                         | `$env.PATH` (Non-Windows) ou `$env.Path` (Windows)            | Voir le chemin courant                                           |
| `echo $?`                            | `$env.LAST_EXIT_CODE`                                         | Voir l'état de sortie de la dernière commande exécutée           |
| `<update ~/.bashrc>`                 | `vim $nu.config-path`                                         | Mettre à jour le chemin de manière permanente                    |
| `export PATH = $PATH:/usr/other/bin` | `$env.PATH = ($env.PATH \| append /usr/other/bin)`            | Mettre à jour le chemin temporairement                           |
| `export`                             | `$env`                                                        | Lister les variables d'environnement courantes                   |
| `<update ~/.bashrc>`                 | `vim $nu.config-path`                                         | Mettre à jour les variables d'environnement de manière permanente |
| `FOO=BAR ./bin`                      | `FOO=BAR ./bin`                                               | Mettre à jour l'environnement temporairement                     |
| `export FOO=BAR`                     | `$env.FOO = BAR`                                              | Définir une variable d'environnement pour la session courante     |
| `echo $FOO`                          | `$env.FOO`                                                    | Utiliser des variables d'environnement                           |
| `echo ${FOO:-fallback}`              | `$env.FOO? \| default "ABC"`                                  | Utiliser une valeur par défaut pour une variable non définie     |
| `unset FOO`                          | `hide-env FOO`                                                | Annuler la définition d'une variable d'environnement pour la session courante |
| `alias s="git status -sb"`           | `alias s = git status -sb`                                    | Définir un alias temporairement                                  |
| `type FOO`                           | `which FOO`                                                   | Afficher les informations sur une commande (intégré, alias ou exécutable) |
| `<update ~/.bashrc>`                 | `vim $nu.config-path`                                         | Ajouter et éditer un alias de manière permanente (pour les nouveaux shells) |
| `bash -c <commands>`                 | `nu -c <commands>`                                            | Exécuter un pipeline de commandes                                |
| `bash <script file>`                 | `nu <script file>`                                            | Exécuter un fichier de script                                    |
| `\`                                  | `( <command> )`                                               | Une commande peut s'étendre sur plusieurs lignes quand enveloppée avec `(` et `)` |
| `pwd` ou `echo $PWD`                 | `pwd` ou `$env.PWD`                                           | Afficher le répertoire courant                                   |
| `read var`                           | `let var = input`                                             | Obtenir l'entrée de l'utilisateur                                |
| `read -s secret`                     | `let secret = input -s`                                       | Obtenir une valeur secrète de l'utilisateur sans imprimer les touches |

## Substitutions historiques et liaisons clavier par défaut :

| Bash                                 | Nu                                                            | Tâche                                                              |
| ------------------------------------ | ------------------------------------------------------------- | ----------------------------------------------------------------- |
| `!!`                                 | `!!`                                                          | Insérer la dernière ligne de commande de l'historique              |
| `!$`                                 | `!$`                                                          | Insérer le dernier jeton spatialement séparé de l'historique      |
| `!<n>` (ex : `!5`)                   | `!<n>`                                                        | Insérer la \<n\>e commande du début de l'historique              |
|                                      |                                                               | Conseil : `history \| enumerate \| last 10` pour montrer les positions récentes |
| `!<-n>` (ex : `!-5`)                 | `!<-n>`                                                       | Insérer la \<n\>e commande de la fin de l'historique             |
| `!<string>` (ex : `!ls`)             | `!<string>`                                                   | Insérer l'élément d'historique le plus récent qui commence par la chaîne |
| <kbd>Ctrl/Cmd</kbd>+<kbd>R</kbd>     | <kbd>Ctrl/Cmd</kbd>+<kbd>R</kbd>                              | Recherche d'historique inverse                                   |
| (Mode Emacs) <kbd>Ctrl</kbd>+<kbd>X</kbd><kbd>Ctrl</kbd>+<kbd>E</kbd> | <kbd>Ctrl/Cmd</kbd>+<kbd>O</kbd> | Éditer la ligne de commande dans l'éditeur défini par `$env.EDITOR` |
| (Mode Vi Command) <kbd>V</kbd>       | <kbd>Ctrl/Cmd</kbd>+<kbd>O</kbd>                              | Éditer la ligne de commande dans l'éditeur défini par `$env.EDITOR` |

La plupart des liaisons clavier communes en mode Emacs et en mode Vi sont également disponibles. Voir le [Chapitre Reedline](line_editor.html#editing-mode).

::: tip
Dans Bash, la substitution historique se produit immédiatement après avoir appuyé sur <kbd>Enter</kbd> 
pour exécuter la ligne de commande. Nushell, cependant, *insère* la substitution dans
la ligne de commande après avoir appuyé sur <kbd>Enter</kbd>. Cela vous permet de confirmer
la substitution et, si nécessaire, de faire des modifications supplémentaires avant l'exécution.

Ce comportement s'étend également à « Éditer la ligne de commande dans l'éditeur ». Tandis que Bash exécute immédiatement
la commande après avoir quitté l'éditeur, Nushell (comme d'autres shells plus modernes
tels que Fish et Zsh) insère le contenu de l'éditeur dans la ligne de commande, ce qui vous permet
d'examiner et de faire des modifications avant de le valider pour l'exécution.
:::
