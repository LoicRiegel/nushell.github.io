---
next:
  text: Venir à Nu
  link: /book/coming_to_nu.md
---

# Tâches de fond

Nushell a actuellement un support expérimental pour les tâches de fond basées sur des threads.

## Lancer des tâches

Les tâches peuvent être lancées en utilisant [`job spawn`](/commands/docs/job_spawn.md), qui reçoit une fermeture et démarre son exécution dans un thread de fond, retournant
un identifiant entier unique pour la tâche lancée :

```nu
'i am' | save status.txt

job spawn { sleep 10sec; ' inevitable' | save --append status.txt }
# => 1

open status.txt
# => i am

# attendre 10 secondes
sleep 10sec

open status.txt
# => i am inevitable
```

## Lister et arrêter les tâches

Les tâches actives peuvent être interrogées avec la commande [`job list`](/commands/docs/job_list.md), qui retourne un tableau avec les informations des tâches en cours d'exécution.
Les tâches peuvent également être tuées/interrompues en utilisant la commande [`job kill`](/commands/docs/job_kill.md), qui interrompt le thread de la tâche et tue tous les processus enfants de la tâche :

```nu
let id = job spawn { sleep 1day }

job list
# => ┏━━━┳━━━━┳━━━━━━━━┳━━━━━━━━━━━━━━━━┓
# => ┃ # ┃ id ┃  type  ┃      pids      ┃
# => ┣━━━╋━━━━╋━━━━━━━━╋━━━━━━━━━━━━━━━━┫
# => ┃ 0 ┃  1 ┃ thread ┃ [list 0 items] ┃
# => ┗━━━┻━━━━┻━━━━━━━━┻━━━━━━━━━━━━━━━━┛

job kill $id

job list
# => ╭────────────╮
# => │ empty list │
# => ╰────────────╯
```

## Suspension de tâches

Sur les cibles Unix, comme Linux et macOS, Nushell supporte également la suspension des commandes externes en utilisant <kbd>Ctrl</kbd>+<kbd>Z</kbd>. Quand un processus en cours d'exécution est suspendu, il devient une tâche de fond « gelée » :

```nu
long_running_process # cela commence à s'exécuter, puis Ctrl+Z est appuyé
# => Job 1 is frozen

job list
# => ┏━━━┳━━━━┳━━━━━━━━┳━━━━━━━━━━━━━━━━┓
# => ┃ # ┃ id ┃  type  ┃      pids      ┃
# => ┣━━━╋━━━━╋━━━━━━━━╋━━━━━━━━━━━━━━━━┫
# => ┃ 0 ┃  1 ┃ frozen ┃ [list 1 items] ┃
# => ┗━━━┻━━━━┻━━━━━━━━┻━━━━━━━━━━━━━━━━┛
```

Une tâche gelée peut être ramenée au premier plan avec la commande [`job unfreeze`](/commands/docs/job_unfreeze.md) :

```nu
job unfreeze
# le processus est ramené à l'endroit où il s'était arrêté
```

::: tip Conseil
Pour ceux qui connaissent d'autres shells Unix, vous pouvez créer un alias pour émuler le comportement de la commande `fg` :

```nu
alias fg = job unfreeze
```

:::

Par défaut, `job unfreeze` va dégeler la tâche la plus récemment gelée. Cependant, vous pouvez aussi spécifier l'identifiant d'une tâche spécifique à dégeler :

```nu
vim
# => Job 1 is frozen

long_running_process
# => Job 2 is frozen

job unfreeze 1
# nous sommes de retour dans vim
```

## Communication entre les tâches

Les données peuvent être envoyées à une tâche en utilisant `job send <id>`, et la tâche peut les recevoir en utilisant `job recv` :

```nu
let jobId = job spawn {
    job recv | save sent.txt
}

'hello from the main thread' | job send $jobId

sleep 1sec

open sent.txt
# => hello from the main thread
```

Le thread principal a un ID de tâche de 0, donc vous pouvez aussi envoyer des données dans l'autre direction :

```nu
job spawn {
    sleep 1sec
    'Hello from a background job' | job send 0
}

job recv
# => Hello from a background job
```

## Comportement à la sortie

Contrairement à de nombreux autres shells, les tâches Nushell ne sont **pas** des processus séparés,
mais sont plutôt implémentées comme des threads de fond.

Un effet secondaire important de cela est que toutes les tâches de fond se termineront une fois que le processus shell se termine.
Pour cette raison, Nushell n'a pas de commande `disown` de type UNIX pour empêcher les tâches de se terminer une fois que le shell se termine.
Pour tenir compte de cela, il y a des plans pour une implémentation `job dispatch` dans le futur,
pour lancer des processus de fond indépendants (voir [#15201](https://github.com/nushell/nushell/issues/15193?issue=nushell%7Cnushell%7C15201) pour les progrès).

De plus, si l'utilisateur exécute une session Nushell interactive et exécute
[`exit`](/commands/docs/exit.md) alors qu'il y a des tâches de fond en cours d'exécution,
le shell avertira l'utilisateur à ce sujet avant de lui demander de confirmer `exit`.
