# Comment configurer des invites de commande tierces

## Nerd Fonts

Les polices Nerd Font ne sont pas obligatoires, mais elles peuvent améliorer la présentation de l'invite de commande grâce à des glyphes et des icônes supplémentaires.

> Nerd Fonts ajoute des correctifs aux polices destinées aux développeurs avec un grand nombre de glyphes (icônes).
> Spécifiquement pour ajouter un grand nombre de glyphes supplémentaires à partir de polices iconiques populaires telles que Font Awesome, Devicons, Octicons et autres.

- [Site web Nerd Fonts](https://www.nerdfonts.com)
- [Dépôt source](https://github.com/ryanoasis/nerd-fonts)

## oh-my-posh

[site](https://ohmyposh.dev/)

[repo](https://github.com/JanDeDobbeleer/oh-my-posh)

Si vous aimez [oh-my-posh](https://ohmyposh.dev/), vous pouvez l'utiliser avec Nushell en quelques étapes. Il fonctionne très bien avec Nushell. Comment configurer oh-my-posh avec Nushell :

1. Installez Oh My Posh et téléchargez les thèmes d'oh-my-posh en suivant le [guide](https://ohmyposh.dev/docs/installation/linux).
2. Téléchargez et installez une [police Nerd Font](https://github.com/ryanoasis/nerd-fonts).
3. Générez le fichier .oh-my-posh.nu. Par défaut, il sera généré dans votre répertoire personnel. Vous pouvez utiliser `--config` pour spécifier un thème, sinon oh-my-posh utilise un thème par défaut.
4. Initialisez l'invite de commande oh-my-posh en ajoutant dans ~/.config/nushell/config.nu (ou le chemin affichée par `$nu.config-path`) pour sourcer ~/.oh-my-posh.nu.

```nu
# Générez le fichier .oh-my-posh.nu
oh-my-posh init nu --config ~/.poshthemes/M365Princess.omp.json

# Initialisez oh-my-posh.nu au démarrage du shell en ajoutant cette ligne dans votre fichier config.nu
source ~/.oh-my-posh.nu
```

Pour les utilisateurs de macOS :

1. Vous pouvez installer oh-my-posh en utilisant `brew`, en suivant simplement le [guide ici](https://ohmyposh.dev/docs/installation/macos)
2. Téléchargez et installez une [police Nerd Font](https://github.com/ryanoasis/nerd-fonts).
3. Définissez PROMPT_COMMAND dans le fichier affichée par `$nu.config-path`, voici un extrait de code :

```nu
let posh_dir = (brew --prefix oh-my-posh | str trim)
let posh_theme = $'($posh_dir)/share/oh-my-posh/themes/'
# Changez les noms de thème en : zash/space/robbyrussel/powerline/powerlevel10k_lean/
# material/half-life/lambda Ou thèmes à double ligne : amro/pure/spaceship, etc.
# Pour plus de [démo des thèmes](https://ohmyposh.dev/docs/themes)
$env.PROMPT_COMMAND = { || oh-my-posh prompt print primary --config $'($posh_theme)/zash.omp.json' }
# Optionnel
$env.PROMPT_INDICATOR = $"(ansi y)$> (ansi reset)"
```

## Starship

[site](https://starship.rs/)

[repo](https://github.com/starship/starship)

1. Suivez les liens ci-dessus et installez Starship.
2. Installez les polices Nerd Font selon vos préférences.
3. Utilisez l'exemple de configuration ci-dessous. Assurez-vous de définir la variable d'environnement `STARSHIP_SHELL`.

::: tip
Une autre façon d'activer Starship est décrite dans les instructions [Installation rapide de Starship](https://starship.rs/#nushell).

Le lien ci-dessus est l'intégration officielle de Starship et Nushell et est le moyen le plus simple de faire fonctionner Starship sans rien faire de manuel :

- Starship créera sa propre configuration / script de configuration d'environnement
- vous devez simplement le créer dans `env.nu` et l'utiliser dans `config.nu`

:::

Voici un exemple de section de configuration pour Starship :

```nu
$env.STARSHIP_SHELL = "nu"

def create_left_prompt [] {
    starship prompt --cmd-duration $env.CMD_DURATION_MS $'--status=($env.LAST_EXIT_CODE)'
}

# Utilisez les fonctions nushell pour définir votre invite droite et gauche
$env.PROMPT_COMMAND = { || create_left_prompt }
$env.PROMPT_COMMAND_RIGHT = ""

# Les indicateurs d'invite sont des variables d'environnement qui représentent
# l'état de l'invite de commande
$env.PROMPT_INDICATOR = ""
$env.PROMPT_INDICATOR_VI_INSERT = ": "
$env.PROMPT_INDICATOR_VI_NORMAL = "〉"
$env.PROMPT_MULTILINE_INDICATOR = "::: "
```

Redémarrez maintenant Nu.

```
nushell on 📙 main is 📦 v0.60.0 via 🦀 v1.59.0
❯
```

## Purs

[repo](https://github.com/xcambar/purs)
