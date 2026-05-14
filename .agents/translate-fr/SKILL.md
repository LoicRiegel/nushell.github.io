---
name: translate-fr
description: French translation workflow for nushell.github.io
---

# When to use

Use this workflow when updating the French documentation in `fr/`.

# Preparation

## General instructions

General translation and contribution rules are documented in `CONTRIBUTING.md`. Load and follow that file first.

## Paths

- English sources: `README.md` and `book/*.md`
- French translations: `fr/README.md` and `fr/book/*.md`
- Translation metadata: `i18n-meta.json`
- Translation helper script: `tools/i18n.nu`
- VuePress config: `.vuepress/config.js`
- VuePress FR navbar config: `.vuepress/configs/navbar/fr.ts`
- VuePress FR sidebar config: `.vuepress/configs/sidebar/fr.ts`

# Workflow

## Step 1: Find outdated files

As instructing in the contributing documentation, run this and read the output to see which files should be updated and/or created.

```sh
nu tools/i18n.nu outdated fr
```

## Step 2: Update VuePress config

For each file detected in step 1: check if a French translation already exists. If not, create it in the appropriate folder. As stated in the contributing guidelines, keep the same file name as the English source.
Keep the same folder structure as the English sources.
Leave the files that already exist unchanged for now.

Update the VuePress config: add the newly created files to the sidebar, the navbar. Do you update the VuePress config.

## Step 3: Update translations

Using the following rules, update and/or create the French translations, given the following rules.

Translation rules:

- Keep code blocks, commands, flags, and links unchanged.
- All the words do not have to be translated. When a term does not have a widely used and known equivalent in French, keep the English term
- Keep terminology consistent across pages, and use the glossary below as the source of truth for terminology consistency
- Preserve markdown structure and headings
- Do not add, remove, or reorder sections unless the English source changed.
- Ensure markdown formatting is preserved
- Translate meaning, not sentence structure
- Prefer clear and technical wording over formal or literary French
- Do not invent missing content. The French translation must match the English source

Translate only the files detected in step 1. Do not modify any other file (including the English sources).
Process files one by one until all outdated files are updated.
Do not stop after translating a single file.

**Kept in English (do not translate):**
shell
pipeline
record
plugin
script
glob
string
closure
scope
runtime
flag
wildcard
backtick
shadowing
release
parsing
toolchain
namespace
cookbook

**Translate consistently:**
command → commande
table → tableau
list → liste
file → fichier
directory / folder → répertoire / dossier
path → chemin
block → bloc
environment → environnement
environment variable → variable d'environnement
built-in → intégré(e)
mutable → mutable (kept as-is, do NOT translate to "modifiable")
immutable → immuable
constant → constante
operator → opérateur
custom command → commande personnalisée
filesystem → système de fichiers
data type → type de donnée
compiled → compilé(e)
binary(ies) → binaire(s)
package manager → gestionnaire de paquets
return value → valeur de retour
parameter → paramètre
argument → argument (when talking about functions)
module → module
syntax → syntaxe

## Step 3: Review

Review your changes and make sure all the rules described in step 2 were followed.
Make sure the new translations did not introduce inconsistencies (the same english term translated in different ways).
If you a new technical term is frequently used, add its translation to the glossary in this file's glossary (in step 2).
If you decided to keep a new term in English in the French translations, also add it in this file's glossary (in step 2).
