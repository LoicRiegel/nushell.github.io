---
next:
  text: Notes de conception
  link: /book/design_notes.md
---

# Carte des opérateurs Nushell

L'idée derrière ce tableau est de vous aider à comprendre comment les opérateurs Nushell se rapportent à d'autres opérateurs de langage. Nous avons essayé de produire une carte de tous les opérateurs nushell et quels sont leurs équivalents dans d'autres langages. Les contributions sont bienvenues.

Remarque : ce tableau suppose Nushell 0.14.1 ou version ultérieure.

| Nushell | SQL      | Python             | .NET LINQ (C#)       | PowerShell             | Bash               |
| ------- | -------- | ------------------ | -------------------- | ---------------------- | ------------------ |
| ==      | =        | ==                 | ==                   | -eq, -is               | -eq                |
| !=      | !=, <>   | !=                 | !=                   | -ne, -isnot            | -ne                |
| <       | <        | <                  | <                    | -lt                    | -lt                |
| <=      | <=       | <=                 | <=                   | -le                    | -le                |
| >       | >        | >                  | >                    | -gt                    | -gt                |
| >=      | >=       | >=                 | >=                   | -ge                    | -ge                |
| =~      | like     | re, in, startswith | Contains, StartsWith | -like, -contains       | =~                 |
| !~      | not like | not in             | Except               | -notlike, -notcontains | ! "str1" =~ "str2" |
| +       | +        | +                  | +                    | +                      | +                  |
| -       | -        | -                  | -                    | -                      | -                  |
| \*      | \*       | \*                 | \*                   | \*                     | \*                 |
| /       | /        | /                  | /                    | /                      | /                  |
| \*\*    | pow      | \*\*               | Power                | Pow                    | \*\*               |
| in      | in       | re, in, startswith | Contains, StartsWith | -In                    | case in            |
| not-in  | not in   | not in             | Except               | -NotIn                 |                    |
| and     | and      | and                | &&                   | -And, &&               | -a, &&             |
| or      | or       | or                 | \|\|                 | -Or, \|\|              | -o, \|\|           |
