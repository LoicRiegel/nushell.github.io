# Scripts

Dans Nushell, vous pouvez écrire et exécuter des scripts dans le langage Nushell. Pour exécuter un script, vous pouvez le passer en argument à l'application de ligne de commande `nu` :

```nu
nu myscript.nu
```

Cela exécutera le script jusqu'à la fin dans une nouvelle instance de Nushell. Vous pouvez également exécuter des scripts à l'intérieur de l'instance _actuelle_ de Nushell en utilisant [`source`](/commands/docs/source.md) :

```nu
source myscript.nu
```

Regardons un exemple de fichier script :

```nu
# myscript.nu
def greet [name] {
  ["hello" $name]
}

greet "world"
```

Un fichier script définit les définitions des commandes personnalisées ainsi que le script principal lui-même, qui s'exécutera après que les commandes personnalisées soient définies.

Dans l'exemple ci-dessus, d'abord `greet` est défini par l'interpréteur Nushell. Cela nous permet d'appeler cette définition plus tard. Nous aurions pu écrire ce qui précède comme :

```nu
greet "world"

def greet [name] {
  ["hello" $name]
}
```

Il n'y a aucune exigence que les définitions doivent venir avant les parties du script qui les appellent, vous permettant de les mettre où vous vous sentez à l'aise.

## Comment les scripts sont traités

Dans un script, les définitions s'exécutent d'abord. Cela nous permet d'appeler les définitions en utilisant les appels du script.

Après l'exécution des définitions, nous commençons en haut du fichier script et exécutons chaque groupe de commandes l'un après l'autre.

## Lignes de script

Pour mieux comprendre comment Nushell voit les lignes de code, regardons un exemple de script :

```nu

```
