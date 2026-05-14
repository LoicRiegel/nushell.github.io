# Variables

Les valeurs Nushell peuvent être assignées à des variables nommées en utilisant les mots-clés `let`, `const` ou `mut`.
Après avoir créé une variable, nous pouvons y faire référence en utilisant `$` suivi de son nom.

## Types de variables

### Variables immuables

Une variable immuable ne peut pas changer sa valeur après la déclaration. Elles sont déclarées en utilisant le mot-clé `let`,

```nu
let val = 42
$val
# => 42
$val = 100
# => Error: nu::shell::assignment_requires_mutable_variable
# =>
# =>   × Assignment to an immutable variable.
# =>    ╭─[entry #10:1:1]
# =>  1 │ $val = 100
# =>    · ──┬─
# =>    ·   ╰── needs to be a mutable variable
# =>    ╰────
```

Cependant, les variables immuables peuvent être « shadowed ». Shadowing signifie qu'elles sont redéclarées et leur valeur initiale ne peut plus être utilisée dans la même scope.

```nu
let val = 42                   # declare a variable
do { let val = 101;  $val }    # in an inner scope, shadow the variable
# => 101
$val                           # in the outer scope the variable remains unchanged
# => 42
let val = $val + 1             # now, in the outer scope, shadow the original variable
$val                           # in the outer scope, the variable is now shadowed, and
# => 43                               # its original value is no longer available.
```

### Variables mutables

Une variable mutable est autorisée à changer sa valeur par assignation. Celles-ci sont déclarées en utilisant le mot-clé `mut`.

```nu
mut val = 42
$val += 27
$val
# => 69
```
