# Overlays

Les overlays agissent comme des « couches » de définitions (commandes personnalisées, alias, variables d'environnement) qui peuvent être activées et désactivées à la demande.
Ils ressemblent à des environnements virtuels trouvés dans certains langages, comme Python.

_Remarque : Pour comprendre les overlays, assurez-vous de consulter [Modules](modules.md) d'abord car les overlays sont construits au-dessus des modules._

## Bases

Tout d'abord, Nushell est livré avec un overlay par défaut appelé `zero`.
Vous pouvez vérifier quels overlays sont actifs avec la commande [`overlay list`](/commands/docs/overlay_list.md).
Vous devriez voir l'overlay par défaut listé là.

Pour créer un nouveau overlay, vous devez d'abord avoir un module :

```nu
module spam {
    export def foo [] {
        "foo"
    }

    export alias bar = echo "bar"

    export-env {
        load-env { BAZ: "baz" }
    }
}
```

Nous utiliserons ce module tout au long du chapitre, donc chaque fois que vous voyez `overlay use spam`, supposez que `spam` se réfère à ce module.

::: tip
Le module peut être créé par l'une des trois méthodes décrites dans [Modules](modules.md) :

- modules « inline » (utilisés dans cet exemple)
- fichier
- répertoire
  :::

Pour créer l'overlay, appelez [`overlay use`](/commands/docs/overlay_use.md) :

```nu
overlay use spam

foo
# => foo

bar
# => bar
```
