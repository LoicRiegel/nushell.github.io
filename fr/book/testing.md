# Test de votre code Nushell

## Commandes d'assertion

Nushell fournit un ensemble de commandes « d'assertion » dans la bibliothèque standard.
On pourrait utiliser des tests d'égalité / ordre intégrés tels que `==` ou `<=` ou des commandes plus complexes et lancer des erreurs manuellement quand une condition attendue échoue, mais utiliser ce que la bibliothèque standard a à offrir est probablement plus facile!

Ce qui suit suppose que le module `std assert` a été importé dans la scope actuelle

```nu
use std/assert
```

Le fondement de chaque assertion est la commande `std assert`. Si la condition n'est pas vraie, elle provoque une erreur.

```nu
assert (1 == 2)
```

```
Error:
  × Assertion failed.
   ╭─[entry #13:1:1]
 1 │ assert (1 == 2)
   ·         ───┬──
   ·            ╰── It is not true.
   ╰────
```

En option, un message peut être défini pour montrer l'intention de la commande assert, ce qui s'est mal passé ou ce qui était attendu :

```nu
let a = 0
assert ($a == 19) $"The lockout code is wrong, received: ($a)"
```

```
Error:
  × The lockout code is wrong, received: 13
   ╭─[entry #25:1:1]
 1 │ let a = 0
 2 │ assert ($a == 19) $"The lockout code is wrong, received: ($a)"
   ·         ────┬───
   ·             ╰── It is not true.
   ╰────
```

Il existe de nombreuses commandes assert, qui se comportent exactement comme celle de base avec l'opérateur approprié. La valeur supplémentaire pour elles est la possibilité de meilleurs messages d'erreur.

Par exemple, ceci n'est pas si utile sans message supplémentaire :
