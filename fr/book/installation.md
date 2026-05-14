---
next:
  text: Default Shell
  link: /book/default_shell.md
---

# Installer Nu

Il existe de nombreuses façons de démarrer avec Nu. Vous pouvez télécharger des binaires précompilés depuis notre [page de release](https://github.com/nushell/nushell/releases), [utiliser votre gestionnaire de paquets préféré](https://repology.org/project/nushell/versions), ou compiler à partir des sources.

Le binaire principal de Nushell est nommé `nu` (ou `nu.exe` sous Windows). Après installation, vous pouvez le lancer en tapant `nu`.

@[code](@snippets/installation/run_nu.sh)

[[toc]]

## Binaires précompilés

Les binaires de Nu sont publiés pour Linux, macOS et Windows [avec chaque release sur GitHub](https://github.com/nushell/nushell/releases). Il vous suffit de les télécharger, d'extraire les binaires, puis de les copier à un emplacement sur votre PATH.

## Gestionnaires de paquets

Nu est disponible via plusieurs gestionnaires de paquets :

[![Statut du packaging](https://repology.org/badge/vertical-allrepos/nushell.svg)](https://repology.org/project/nushell/versions)

Pour macOS et Linux, [Homebrew](https://brew.sh/) est un choix populaire (`brew install nushell`).

Pour Windows :

- [Winget](https://docs.microsoft.com/fr-fr/windows/package-manager/winget/)

  - Installation avec portée machine : `winget install nushell --scope machine`
  - Mise à jour avec portée machine : `winget update nushell`
  - Installation avec portée utilisateur : `winget install nushell` ou `winget install nushell --scope user`
  - Mise à jour avec portée utilisateur : En raison du [problème winget-cli #3011](https://github.com/microsoft/winget-cli/issues/3011), exécuter `winget update nushell` installera inopinément la dernière version dans `C:\Program Files\nu`. Pour contourner cela, relancez `winget install nushell` pour installer la dernière version dans la portée utilisateur.

- [Scoop](https://scoop.sh/) (`scoop install nu`)

Pour Debian et Ubuntu :

```sh
wget -qO- https://apt.fury.io/nushell/gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/fury-nushell.gpg
echo "deb [signed-by=/etc/apt/keyrings/fury-nushell.gpg] https://apt.fury.io/nushell/ /" | sudo tee /etc/apt/sources.list.d/fury-nushell.list
sudo apt update
sudo apt install nushell
```

Pour RedHat/Fedora et Rocky Linux :

```sh
echo "[gemfury-nushell]
name=Gemfury Nushell Repo
baseurl=https://yum.fury.io/nushell/
enabled=1
gpgcheck=0
gpgkey=https://yum.fury.io/nushell/gpg.key" | sudo tee /etc/yum.repos.d/fury-nushell.repo
sudo dnf install -y nushell
```

Pour Alpine Linux :

```sh
echo "https://alpine.fury.io/nushell/" | tee -a /etc/apk/repositories
apk update
apk add --allow-untrusted nushell
```

Installation multiplateforme :

- [npm](https://www.npmjs.com/) (`npm install -g nushell` — Notez que les plugins Nu ne sont pas inclus si vous installez de cette manière)

## Images Docker

Les images Docker sont disponibles depuis le GitHub Container Registry. Une image pour la dernière release est régulièrement construite pour Alpine et Debian. Vous pouvez exécuter l'image en mode interactif avec :

```nu
docker run -it --rm ghcr.io/nushell/nushell:<version>-<distro>
```

Où `<version>` est la version de Nushell à exécuter et `<distro>` est `alpine` ou la dernière release Debian supportée, par exemple `bookworm`.

Pour exécuter une commande spécifique :

```nu
docker run --rm ghcr.io/nushell/nushell:latest-alpine -c "ls /usr/bin | where size > 10KiB"
```

Pour exécuter un script depuis le répertoire courant via Bash :

```nu
docker run --rm \
    -v $(pwd):/work \
    ghcr.io/nushell/nushell:latest-alpine \
    "/work/script.nu"
```

## Compiler à partir des sources

Vous pouvez également compiler Nu à partir des sources. Tout d'abord, vous devrez configurer la toolchain Rust et ses dépendances.

### Installation d'une suite de compilateurs

Pour que Rust fonctionne correctement, vous devrez avoir une suite de compilateurs compatible installée sur votre système. Les suites de compilateurs recommandées sont :

- Linux : GCC ou Clang
- macOS : Clang (installez Xcode)
- Windows : MSVC (installez [Visual Studio](https://visualstudio.microsoft.com/vs/community/) ou les [Visual Studio Build Tools](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022))
  - Assurez-vous d'installer le workload "Desktop development with C++"
  - Toute édition de Visual Studio fonctionnera (Community est gratuite)

### Installation de Rust

Si vous n'avez pas encore Rust sur votre système, la meilleure façon de l'installer est via [rustup](https://rustup.rs/). Rustup est un outil de gestion des installations de Rust, y compris la gestion de l'utilisation de différentes versions de Rust.

Nu nécessite actuellement la **dernière version stable (1.66.1 ou plus récente)** de Rust. Le mieux est de laisser `rustup` trouver la version correcte pour vous. Lorsque vous ouvrez `rustup` pour la première fois, il vous demandera quelle version de Rust vous souhaitez installer :

@[code](@snippets/installation/rustup_choose_rust_version.sh)

Une fois prêt, appuyez sur 1 puis sur Entrée.

Si vous préférez ne pas installer Rust via `rustup`, vous pouvez également l'installer via d'autres méthodes (par exemple, depuis un paquet dans une distro Linux). Assurez-vous simplement d'installer une version de Rust qui soit 1.66.1 ou plus récente.

### Dépendances

#### Debian/Ubuntu

Vous devrez installer les paquets "pkg-config", "build-essential" et "libssl-dev" :

@[code](@snippets/installation/install_pkg_config_libssl_dev.sh)

#### Distros basées sur RHEL

Vous devrez installer "libxcb", "openssl-devel" et "libX11-devel" :

@[code](@snippets/installation/install_rhel_dependencies.sh)

#### macOS

##### Homebrew

En utilisant [Homebrew](https://brew.sh/), vous devrez installer "openssl" et "cmake" en utilisant :

@[code](@snippets/installation/macos_deps.sh)

##### Nix

Si vous utilisez [Nix](https://nixos.org/download/#nix-install-macos) pour la gestion de paquets sur macOS, les paquets `openssl`, `cmake`, `pkg-config` et `curl` sont requis. Ils peuvent être installés :

- Globalement, avec `nix-env --install` (et d'autres).
- Localement, avec [Home Manager](https://github.com/nix-community/home-manager) dans votre config `home.nix`.
- Temporairement, avec `nix-shell` (et d'autres).

### Compiler depuis [crates.io](https://crates.io) avec Cargo

Les releases de Nushell sont publiées sur le populaire gestionnaire de paquets Rust [crates.io](https://crates.io/). Cela facilite la compilation et l'installation de la dernière release de Nu avec `cargo` :

```nu
cargo install nu --locked
```

L'outil `cargo` se chargera de télécharger les sources de Nu et de ses dépendances, de les compiler, et d'installer Nu à l'emplacement des binaires de cargo.

Notez que les plugins par défaut doivent être installés séparément avec `cargo`. Voir la section [Installation des Plugins](./plugins.html#core-plugins) du livre pour les instructions.

### Compiler à partir du dépôt GitHub

Vous pouvez également compiler Nu à partir des dernières sources sur GitHub. Cela vous donne un accès immédiat aux dernières fonctionnalités et corrections de bugs. Tout d'abord, clonez le dépôt :

@[code](@snippets/installation/git_clone_nu.sh)

Nous pouvons à présent compiler et exécuter Nu avec :

@[code](@snippets/installation/build_nu_from_source.sh)

Vous pouvez également compiler et exécuter Nu en mode release, ce qui active plus d'optimisations :

@[code](@snippets/installation/build_nu_from_source_release.sh)

Les personnes familières avec Rust pourraient se demander pourquoi nous faisons une étape de "build" et une étape de "run" séparément alors que "run" effectue une compilation par défaut. Cela permet de contourner une limitation de la nouvelle option `default-run` dans Cargo, et de s'assurer que tous les plugins sont bien compilés, bien que cela ne soit peut-être plus nécessaire à l'avenir.
