# Plugins

Nu peut être étendu en utilisant des plugins. Les plugins se comportent beaucoup comme les commandes intégrées de Nushell, avec l'avantage supplémentaire qu'ils peuvent être ajoutés séparément de Nu lui-même.

::: warning Important
Les plugins communiquent avec Nushell en utilisant le protocole `nu-plugin`. Ce protocole est versionnié, et les plugins doivent utiliser la même version `nu-plugin` fournie par Nushell.

Lors de la mise à jour de Nushell, veuillez vous assurer de mettre à jour également tous les plugins que vous avez enregistrés.
:::

[[toc]]

## Aperçu

- Pour utiliser un plugin, il doit être :

  - Installé
  - Ajouté
  - Importé

Il existe deux types de plugins :

- Les « plugins centraux » sont officiellement maintenus et sont généralement installés avec Nushell, dans le même répertoire que l'exécutable Nushell.
- Des plugins tiers sont également disponibles de nombreuses sources.

La constante `$NU_LIB_DIRS` ou la variable d'environnement `$env.NU_LIB_DIRS` peut être utilisée pour définir le chemin de recherche des plugins.

### Démarrage rapide du plugin central

Pour commencer à utiliser le plugin Polars :

1. La plupart des gestionnaires de paquets installeront automatiquement les plugins centraux avec Nushell. Une exception notable, cependant, est `cargo`. Si vous avez installé Nushell en utilisant `cargo`, voir [Installation des plugins centraux](#core-plugins) ci-dessous.

2. (Recommandé) Définissez le chemin de recherche du plugin pour inclure le répertoire où Nushell et ses plugins sont installés. En supposant que les plugins centraux sont installés dans le même répertoire que le binaire Nushell, ce qui suit peut être ajouté à votre configuration de démarrage :

   ```nu
   const NU_PLUGIN_DIRS = [
     ($nu.current-exe | path dirname)
     ...$NU_PLUGIN_DIRS
   ]
   ```

3. Ajoutez le plugin au registre de plugins. Cela ne doit être fait qu'une seule fois. Le nom est le nom du fichier du plugin, y compris son extension :

   ```nu
   # On Unix/Linux platforms:
   plugin add nu_plugin_polars
   # Or on Windows
   ```
