# Parallélisme

Nushell a maintenant un support précoce pour l'exécution du code en parallèle. Cela vous permet de traiter les éléments d'un stream en utilisant plus de ressources matérielles de votre ordinateur.

Vous remarquerez ces commandes avec leur dénomination caractéristique `par-`. Chacune correspond à une version non parallèle, vous permettant d'écrire facilement du code dans un style série d'abord, puis de revenir et de convertir facilement les scripts série en scripts parallèles avec quelques caractères supplémentaires.

## par-each

La commande parallèle la plus courante est [`par-each`](/commands/docs/par-each.md), une compagne de la commande [`each`](/commands/docs/each.md).

Comme [`each`](/commands/docs/each.md), [`par-each`](/commands/docs/par-each.md) fonctionne sur chaque élément du pipeline au fur et à mesure qu'il arrive, exécutant un bloc sur chacun. Contrairement à [`each`](/commands/docs/each.md), [`par-each`](/commands/docs/par-each.md) fera ces opérations en parallèle.

Disons que vous vouliez compter le nombre de fichiers dans chaque sous-répertoire du répertoire courant. Utilisant [`each`](/commands/docs/each.md), vous pourriez écrire ceci comme :

```nu
ls | where type == dir | each { |row|
    { name: $row.name, len: (ls $row.name | length) }
}
```

Nous créons un enregistrement pour chaque entrée, et le remplissons avec le nom du répertoire et le nombre d'entrées dans ce sous-répertoire.

Sur votre machine, les temps peuvent varier. Pour cette machine, cela a pris 21 millisecondes pour le répertoire courant.

Maintenant, puisque cette opération peut être exécutée en parallèle, convertissons ce qui précède en parallèle en changeant [`each`](/commands/docs/each.md) en [`par-each`](/commands/docs/par-each.md) :

```nu
ls | where type == dir | par-each { |row|
    { name: $row.name, len: (ls $row.name | length) }
}
```

Sur cette machine, cela s'exécute maintenant en 6ms. C'est une belle différence!

À noter : Parce que [les variables d'environnement sont scoped](environment.md#scoping), vous pouvez utiliser [`par-each`](/commands/docs/par-each.md) pour travailler dans plusieurs répertoires en parallèle (remarquez la commande [`cd`](/commands/docs/cd.md)) :

```nu
ls | where type == dir | par-each { |row|
    { name: $row.name, len: (cd $row.name; ls | length) }
}
```

Vous remarquerez, si vous regardez les résultats, qu'ils reviennent dans des ordres différents à chaque exécution (selon le nombre de threads matériels sur votre système). Comme les tâches se terminent et que nous obtenons le résultat correct, nous pouvons avoir besoin d'ajouter des étapes supplémentaires si nous voulons que nos résultats dans un ordre particulier. Par exemple, pour ce qui précède, nous pourrions vouloir trier les résultats par le champ « name ». Cela permet aux versions [`each`](/commands/docs/each.md) et [`par-each`](/commands/docs/par-each.md) de notre script de donner le même résultat.
