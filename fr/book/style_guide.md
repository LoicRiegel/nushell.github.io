---
next:
  text: Nu comme un shell
  link: /book/nu_as_a_shell.md
---

# Meilleures pratiques

Cette page est un document de travail collectant les directives de syntaxe et les meilleures pratiques que nous avons découvertes jusqu'à présent.
L'objectif de ce document est d'éventuellement parvenir à un style de code Nushell canonique, mais pour l'instant, il s'agit toujours d'un travail en
progrès et sujets à changement. Nous accueillons les discussions et les contributions.

Gardez à l'esprit que ces directives ne sont pas obligatoires à utiliser dans les référentiels externes (pas les nôtres), vous pouvez les modifier à la
manière dont vous voulez, mais veuillez être cohérent et suivre vos règles.

Toutes les séquences d'échappement ne doivent pas être interprétées littéralement, sauf indication contraire. En d'autres termes,
traitez quelque chose comme `\n` comme le caractère de nouvelle ligne et non une barre oblique littérale suivie d'un `n`.

## Formatage

### Valeurs par défaut

**Il est recommandé d'** assumer que par défaut aucun espace ou tabulation n'est autorisé, mais les règles suivantes définissent où ils sont autorisés.

### Basique

- **Il est recommandé de** mettre un espace avant et après le symbole du pipe `|`, les commandes, les sous-commandes, leurs options et arguments.
- **Il est recommandé de** ne jamais mettre plusieurs espaces consécutifs sauf s'ils font partie d'une chaîne.
- **Il est recommandé d'** omettre les virgules entre les éléments de la liste.

Correct :

```nu
'Hello, Nushell! This is a gradient.' | ansi gradient --fgstart '0x40c9ff' --fgend '0xe81cff'
```

Incorrect :

```nu
# - too many spaces after "|": 2 instead of 1
'Hello, Nushell! This is a gradient.' |  ansi gradient --fgstart '0x40c9ff' --fgend '0xe81cff'
```

#### Format d'une ligne

Le format d'une ligne est un format pour écrire toutes les commandes sur une seule ligne.

**Il est recommandé** par défaut d'utiliser ce format :

1. sauf si vous écrivez des scripts
2. dans les scripts pour les listes et les enregistrements sauf s'ils :
