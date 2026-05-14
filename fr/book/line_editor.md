# Reedline, l'éditeur de ligne de Nushell

L'éditeur de ligne de Nushell [Reedline](https://github.com/nushell/reedline) est multiplateforme et conçu pour être modulaire et flexible. L'éditeur de ligne est responsable du contrôle de l'historique des commandes, des validations, des complétions, des conseils, de la peinture d'écran, et bien plus.

[[toc]]

## Édition multi-ligne

Reedline permet aux lignes de commande de Nushell de s'étendre sur plusieurs lignes. Cela peut être réalisé de plusieurs façons :

1. Appuyer sur <kbd>Entrée</kbd> quand une expression entre crochets est ouverte.

   Par exemple :

   ```nu
   def my-command [] {
   ```

   Appuyer sur <kbd>Entrée </kbd> après le crochet ouvert insérera une nouvelle ligne. Cela se produira également avec des expressions `(` et `[` valides et ouvertes.

   Ceci est couramment utilisé pour créer des blocs et des fermetures (comme ci-dessus), mais aussi des littéraux de liste, d'enregistrement et de tableau :

   ```nu
   let file = {
     name: 'repos.sqlite'
     hash: 'b939a3fa4ca011ca1aa3548420e78cee'
     version: '1.4.2'
   }
   ```

   Il peut même être utilisé pour continuer une seule commande sur plusieurs lignes :

   ::: details Exemple

   ```nu
   (
     tar
     -cvz
     -f archive.tgz
     --exclude='*.temp'
     --directory=../project/
     ./
   )
   ```

   :::

2. Appuyer sur <kbd>Entrée</kbd> à la fin d'une ligne avec un symbole de tuyau de fin (`|`).

   ```nu
   ls                     |
   where name =~ '^[0-9]' | # Comments after a trailing pipe are okay
   get name               |
   mv ...$in ./backups/
   ```

3. Insérez manuellement une nouvelle ligne en utilisant <kbd>Alt</kbd>+<kbd>Entrée</kbd> ou <kbd>Shift</kbd>+<kbd>Entrée</kbd>.

   Ceci peut être utilisé pour créer une version quelque peu plus lisible de la ligne de commande précédente :

   ```nu
   ls
   | where name =~ '^[0-9]'  # Files starting with a digit
   | get name
   | mv ...$in ./backups/
   ```

   ::: tip
   Il est possible qu'une ou les deux de ces liaisons clavier puissent être interceptées par l'application terminal ou le gestionnaire de fenêtres. Par exemple, Windows Terminal (et la plupart des autres applications de terminal sous Windows) assigne <kbd>Alt</kbd>+<kbd>Entrée</kbd> pour agrandir le terminal en plein écran. Si aucune des liaisons clavier ci-dessus ne fonctionne dans votre terminal, vous pouvez assigner une liaison clavier différente à :

   ```nu
   event: { edit: insertnewline }
   ```

   Voir [Liaisons clavier](#keybindings) ci-dessous pour plus de détails.

   :::
