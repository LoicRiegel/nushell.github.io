# Tri

Nushell offre de nombreuses façons de trier les données, et la méthode que vous choisirez dépendra du problème et du type de données avec lesquelles vous travaillez. Regardons certains des moyens par lesquels vous pourriez souhaiter trier les données.

## Tri de base

### Listes

Le tri d'une liste de base fonctionne exactement comme vous pourriez vous y attendre :

```nu
[9 3 8 1 4 6] | sort
# => ╭───┬───╮
# => │ 0 │ 1 │
# => │ 1 │ 3 │
# => │ 2 │ 4 │
# => │ 3 │ 6 │
# => │ 4 │ 8 │
# => │ 5 │ 9 │
# => ╰───┴───╯
```

Cependant, les choses deviennent un peu plus complexes quand vous commencez à combiner les types. Par exemple, voyons ce qui se passe quand nous avons une liste avec des nombres _et_ des chaînes :

```nu
["hello" 4 9 2 1 "foobar" 8 6] | sort
# => ╭───┬────────╮
# => │ 0 │      1 │
# => │ 1 │      2 │
# => │ 2 │      4 │
# => │ 3 │      6 │
# => │ 4 │      8 │
# => │ 5 │      9 │
# => │ 6 │ foobar │
# => │ 7 │ hello  │
# => ╰───┴────────╯
```

Nous pouvons voir que les nombres sont triés dans l'ordre, et les chaînes sont triées à la fin de la liste, également dans l'ordre. Si vous venez d'autres langages de programmation, ce ne peut pas être exactement ce que vous attendiez. Dans Nushell, en règle générale, **les données peuvent toujours être triées sans erreur**.

::: tip
Si vous _le voulez vraiment_ un tri contenant des types différents pour faire une erreur, voir [tri strict](#strict-sort).
:::

Le tri de Nushell est également **stable**, ce qui signifie que les valeurs égales conserveront leur ordre original les unes par rapport aux autres. Ceci est illustré ici en utilisant l'option de tri [insensible à la casse](#case-insensitive-sort) :

```nu
["foo" "FOO" "BAR" "bar"] | sort -i
# => ╭───┬─────╮
# => │ 0 │ BAR │
```
