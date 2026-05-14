# Venir de PowerShell

::: tip
Les pipelines PowerShell transmettent des **objets .NET riches**, qui permettent l'accès aux propriétés comme `$process.Name` ou de diriger les objets directement dans les cmdlets qui les comprennent.

Les pipelines Nushell, en revanche, transmettent des **données structurées** telles que les tableaux, les listes et les valeurs.
Cela signifie :

- Pas d'accès `.PropertyName`
- Utiliser `get column`, `select`, `$it.column` ou des opérations sur tableau à la place
- Les commandes reçoivent toujours une entrée structurée prévisible, pas des chaînes ou des types .NET
  :::

## Équivalents de commandes :

| PowerShell                                                      | Nu                                           | Tâche                                                          |
| --------------------------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------- |
| `Get-ChildItem`                                                 | `ls`                                         | Lister les fichiers du répertoire courant                      |
| `Get-ChildItem <dir>`                                           | `ls <dir>`                                   | Lister les fichiers du répertoire donné                        |
| `Get-ChildItem pattern*`                                        | `ls pattern*`                                | Correspondance de motif des fichiers                           |
| `Get-ChildItem -Force -File -Hidden`                            | `ls --long --all` ou `ls -la`                | Listing détaillé y compris les fichiers cachés                 |
| `Get-ChildItem \| Where-Object { $_.PSIsContainer }`            | `ls \| where type == dir`                    | Lister uniquement les répertoires                              |
| `Get-ChildItem -Recurse -Filter *.rs`                           | `ls **/*.rs`                                 | Recherche récursive de fichiers                                |
| `Get-ChildItem -Recurse Makefile \| Select-Object -Expand Name` | `ls **/Makefile \| get name \| vim ...$in`   | Passer les chemins appariés à une commande                     |
| `Set-Location <dir>`                                            | `cd <dir>`                                   | Changer de répertoire                                          |
| `Set-Location`                                                  | `cd`                                         | Aller au répertoire personnel                                  |
| `Set-Location -`                                                | `cd -`                                       | Aller au répertoire précédent                                  |
| `New-Item -ItemType Directory <path>`                           | `mkdir <path>`                               | Créer un répertoire                                            |
| `New-Item test.txt`                                             | `touch test.txt`                             | Créer un fichier                                               |
| `command \| Out-File <path>`                                    | `out> <path>` ou `o> <path>`                 | Enregistrer la sortie dans un fichier (brut)                   |
| `command \| Set-Content <path>`                                 | `\| save <path>`                             | Enregistrer la sortie dans un fichier (structuré)              |
| `command \| Out-File -Append <path>`                            | `out>> <path>` ou `o>> <path>`               | Ajouter la sortie au fichier                                   |
|                                                                 | `\| save --append <path>`                    | Ajouter la sortie structurée                                   |
| `command \| Out-Null`                                           | `\| ignore`                                  | Ignorer la sortie                                              |
| `cmd1 \| Tee-Object -FilePath log.txt \| cmd2`                  | `cmd1 \| tee { save log.txt } \| cmd2`       | Tee la sortie vers un fichier                                  |
| `command \| Select-Object -First 5`                             | `command \| first 5`                         | Limiter la sortie aux N premières lignes                       |
| `Get-Content <path>`                                            | `open --raw <path>`                          | Afficher le contenu du fichier                                 |
| `Move-Item <source> <dest>`                                     | `mv <source> <dest>`                         | Déplacer le fichier                                            |
| `Get-ChildItem *.md \| ForEach-Object { $_.Name }`              | `ls *.md \| each { $in.name }`               | Itérer sur les valeurs de la liste                             |
| `foreach ($i in 1..10) { $i }`                                  | `for i in 1..10 { print $i }`                | Boucler sur une plage                                          |
| `Copy-Item <source> <dest>`                                     | `cp <source> <dest>`                         | Copier le fichier                                              |
| `Copy-Item -Recurse <source> <dest>`                            | `cp -r <source> <dest>`                      | Copier le répertoire récursivement                             |
| `Remove-Item <path>`                                            | `rm <path>`                                  | Supprimer le fichier                                           |
|                                                                 | `rm -t <path>`                               | Déplacer le fichier vers la corbeille                          |
| `Remove-Item -Recurse -Force <path>`                            | `rm -r <path>`                               | Supprimer le répertoire récursivement                          |
| `Get-Date "<date>"`                                             | `"<date>" \| into datetime -f <format>`      | Analyser la date                                               |
| `"<str>" -replace 'a','b'`                                      | `str replace "a" "b"`                        | Remplacer les sous-chaînes                                     |
| `Select-String <pattern>`                                       | `where $it =~ <pattern>` ou `find <pattern>` | Texte de recherche                                             |
| `Get-Help <command>`                                            | `help <command>`                             | Obtenir l'aide de la commande                                  |
| `Get-Command`                                                   | `help commands`                              | Lister toutes les commandes                                    |
| `Get-Command "*<string>*"`                                      | `help --find <string>`                       | Commandes de recherche                                         |
| `command1; if ($?) { command2 }`                                | `command1; command2`                         | Exécuter la deuxième commande seulement si la première réussit |
| `/tmp/$((Get-Random))`                                          | `$"/tmp/(random int)"`                       | Interpolation de chaîne                                        |
| `$env:Path`                                                     | `$env.PATH` ou `$env.Path`                   | Afficher le CHEMIN                                             |
| `$LASTEXITCODE`                                                 | `$env.LAST_EXIT_CODE`                        | Code de sortie de la dernière commande externe                 |
| `$env:PATH += ":/usr/bin"`                                      | `$env.PATH = ($env.PATH \| append /usr/bin)` | Mettre à jour le CHEMIN (temporaire)                           |
| `Get-ChildItem Env:`                                            | `$env`                                       | Lister les variables d'environnement                           |
| `$env:FOO`                                                      | `$env.FOO`                                   | Accéder à la variable d'environnement                          |
| `Remove-Item Env:FOO`                                           | `hide-env FOO`                               | Annuler la définition de la variable d'environnement           |
| `Set-Alias s "git status -sb"`                                  | `alias s = git status -sb`                   | Alias temporaire                                               |
| `Get-Command FOO`                                               | `which FOO`                                  | Inspecter la commande / alias / binaire                        |
| `powershell -Command "<commands>"`                              | `nu -c <commands>`                           | Exécuter le pipeline en ligne                                  |
| `.\script.ps1`                                                  | `nu <script file>`                           | Exécuter le fichier de script                                  |
| `Get-Location` ou `$PWD`                                        | `pwd` ou `$env.PWD`                          | Afficher le répertoire courant                                 |
| `Read-Host`                                                     | `let var = input`                            | Lire l'entrée de l'utilisateur                                 |
| `Read-Host -AsSecureString`                                     | `let secret = input -s`                      | Lire l'entrée secrète                                          |

## Effacer le tampon de ligne de commande en appuyant sur ESC

Les versions héritées de PowerShell ont la capacité d'effacer le tampon de ligne de commande en appuyant sur la touche ESC.

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
