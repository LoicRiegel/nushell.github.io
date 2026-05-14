# Venir de CMD.EXE

Ce tableau a été mis à jour pour Nu 0.67.0.

| CMD.EXE                              | Nu                                                                                  | Tâche                                                                                          |
| ------------------------------------ | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `ASSOC`                              |                                                                                     | Affiche ou modifie les associations d'extension de fichier                                     |
| `BREAK`                              |                                                                                     | Déclencher un point d'arrêt du débogueur                                                       |
| `CALL <filename.bat>`                | `<filename.bat>`                                                                    | Exécuter un programme batch                                                                    |
|                                      | `nu <filename>`                                                                     | Exécuter un script nu dans un contexte frais                                                   |
|                                      | `source <filename>`                                                                 | Exécuter un script nu dans ce contexte                                                         |
|                                      | `use <filename>`                                                                    | Exécuter un script nu comme module                                                             |
| `CD` ou `CHDIR`                      | `$env.PWD`                                                                          | Obtenir le répertoire de travail courant                                                       |
| `CD <directory>`                     | `cd <directory>`                                                                    | Changer le répertoire courant                                                                  |
| `CD /D <drive:directory>`            | `cd <drive:directory>`                                                              | Changer le répertoire courant                                                                  |
| `CLS`                                | `clear`                                                                             | Effacer l'écran                                                                                |
| `COLOR`                              |                                                                                     | Définir les couleurs de premier plan/arrière-plan par défaut de la console                     |
|                                      | `ansi {flags} (code)`                                                               | Sortir les codes ANSI pour changer la couleur                                                  |
| `COPY <source> <destination>`        | `cp <source> <destination>`                                                         | Copier les fichiers                                                                            |
| `COPY <file1>+<file2> <destination>` | `[<file1>, <file2>] \| each { open --raw } \| str join \| save --raw <destination>` | Ajouter plusieurs fichiers en un                                                               |
| `DATE /T`                            | `date now`                                                                          | Obtenir la date courante                                                                       |
| `DATE`                               |                                                                                     | Définir la date                                                                                |
| `DEL <file>` ou `ERASE <file>`       | `rm <file>`                                                                         | Supprimer les fichiers                                                                         |
| `DIR`                                | `ls`                                                                                | Lister les fichiers du répertoire courant                                                      |
| `ECHO <message>`                     | `print <message>`                                                                   | Imprimer les valeurs données à stdout                                                          |
| `ECHO ON`                            |                                                                                     | Imprimer les commandes exécutées à stdout                                                      |
| `ENDLOCAL`                           | `export-env`                                                                        | Changer l'env dans l'appelant                                                                  |
| `EXIT`                               | `exit`                                                                              | Fermer l'invite ou le script                                                                   |
| `FOR %<var> IN (<set>) DO <command>` | `for $<var> in <set> { <command> }`                                                 | Exécuter une commande pour chaque élément d'un ensemble                                        |
| `FTYPE`                              |                                                                                     | Affiche ou modifie les types de fichiers utilisés dans les associations d'extension de fichier |
| `GOTO`                               |                                                                                     | Sauter à une étiquette                                                                         |
| `IF ERRORLEVEL <number> <command>`   | `if $env.LAST_EXIT_CODE >= <number> { <command> }`                                  | Exécuter une commande si la dernière commande a renvoyé un code d'erreur >= spécifié           |
| `IF <string> EQU <string> <command>` | `if <string> == <string> { <command> }`                                             | Exécuter une commande si les chaînes correspondent                                             |
| `IF EXIST <filename> <command>`      | `if (<filename> \| path exists) { <command> }`                                      | Exécuter une commande si le fichier existe                                                     |
| `IF DEFINED <variable> <command>`    | `if '$<variable>' in (scope variables).name { <command> }`                          | Exécuter une commande si la variable est définie                                               |
| `MD` ou `MKDIR`                      | `mkdir`                                                                             | Créer des répertoires                                                                          |
| `MKLINK`                             |                                                                                     | Créer des liens symboliques                                                                    |
| `MOVE`                               | `mv`                                                                                | Déplacer les fichiers                                                                          |
| `PATH`                               | `$env.Path`                                                                         | Afficher la variable de chemin courant                                                         |
| `PATH <path>;%PATH%`                 | `$env.Path = ($env.Path \| append <path>`)                                          | Éditer la variable de chemin                                                                   |
| `PATH %PATH%;<path>`                 | `$env.Path = ($env.Path \| prepend <path>`)                                         | Éditer la variable de chemin                                                                   |
| `PAUSE`                              | `input "Press any key to continue . . ."`                                           | Pause l'exécution du script                                                                    |
| `PROMPT <template>`                  | `$env.PROMPT_COMMAND = { <command> }`                                               | Modifier l'invite du terminal                                                                  |
| `PUSHD <path>`/`POPD`                | `enter <path>`/`dexit`                                                              | Changer temporairement de répertoire de travail                                                |
| `REM`                                | `#`                                                                                 | Commentaires                                                                                   |
| `REN` ou `RENAME`                    | `mv`                                                                                | Renommer les fichiers                                                                          |
| `RD` ou `RMDIR`                      | `rm`                                                                                | Supprimer le répertoire                                                                        |
| `SET <var>=<string>`                 | `$env.<var> = <string>`                                                             | Définir les variables d'environnement                                                          |
| `SETLOCAL`                           | (comportement par défaut)                                                           | Localiser les modifications d'environnement dans un script                                     |
| `START <path>`                       | Partiellement couvert par `start <path>`                                            | Ouvrir le chemin dans l'application par défaut configurée par le système                       |
| `START <internal command>`           |                                                                                     | Démarrer une fenêtre séparée pour exécuter une commande interne spécifiée                      |
| `START <batch file>`                 |                                                                                     | Démarrer une fenêtre séparée pour exécuter un fichier batch spécifié                           |
| `TIME /T`                            | `date now \| format date "%H:%M:%S"`                                                | Obtenir l'heure courante                                                                       |
| `TIME`                               |                                                                                     | Définir l'heure courante                                                                       |
| `TITLE`                              |                                                                                     | Définir le nom de la fenêtre cmd.exe                                                           |
| `TYPE`                               | `open --raw`                                                                        | Afficher le contenu d'un fichier texte                                                         |
|                                      | `open`                                                                              | Ouvrir un fichier en tant que données structurées                                              |
| `VER`                                |                                                                                     | Afficher la version du système d'exploitation                                                  |
| `VERIFY`                             |                                                                                     | Vérifier que les écritures de fichier se produisent                                            |
| `VOL`                                |                                                                                     | Afficher les informations du lecteur                                                           |

## Commandes CMD.EXE transférées

Nu accepte et exécute _certaines_ des commandes internes de CMD.EXE via `cmd.exe`.

Les commandes internes sont : `ASSOC`, `CLS`, `ECHO`, `FTYPE`, `MKLINK`, `PAUSE`, `START`, `VER`, `VOL`

Ces commandes internes ont la priorité sur les commandes externes.

Par exemple, avec un fichier `ver.bat` dans le répertoire de travail courant, exécuter `^ver` exécute la commande interne `VER` de CMD.EXE, _PAS_ le fichier `ver.bat`.

Exécuter `./ver` ou `ver.bat` _va_ exécuter le fichier bat local cependant.

Notez que Nushell a sa propre commande [`start`](/commands/docs/start.md) qui a la priorité.
Vous pouvez appeler la commande interne `START` de CMD.EXE avec la syntaxe de commande externe `^start`.

## Effacer le tampon de ligne de commande en appuyant sur ESC

CMD.EXE a la capacité d'effacer le tampon de ligne de commande en appuyant sur la touche ESC.

Bien que Nu ne le fasse pas par défaut, un keybind peut être ajouté à votre `config.nu` pour activer une fonctionnalité similaire :

```nu
$env.config.keybindings ++= [{
    name: 'esc_clear'
    modifier: 'None'
    keycode: 'Esc'
    mode: ['Emacs', 'Vi_Normal']
    event: {edit: 'Clear'}
}]
```
