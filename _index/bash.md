---
tags: [index, bash]
---

# MOC — Bash

Map of Content pour les notes bash.

## Fondamentaux

- [[bash-overview]] — Shell Bourne Again, amélioration du Bourne Shell original
- [[terminal-shell]] — Terminal et shell, notions de base
- [[types-de-shell]] — Panorama des différents shells (sh, csh, ksh, zsh, fish, PowerShell)

## Commandes et builtins

- [[mapfile]] — Lire des lignes dans un tableau bash depuis stdin

## Options et mode strict

- [[set-euo-pipefail]] — Mode strict bash (`-e`, `-u`, `-o pipefail`)

## Expansions et variables

- [[variable-indirection]] — Indirection `${!var}` et expansion de casse `${var^^}`

## Patterns

- [[error-accumulation-pattern]] — Collecter toutes les erreurs avant de quitter

## Parsing texte

- [[grep-o]] — Extraire uniquement la partie correspondante avec `grep -o`, parsing JSON sans jq
- [[tr]] — Transformer ou supprimer des caractères (`tr -d`)

## Regex POSIX

- [[regex-posix]] — Classes `[[:space:]]`, quantificateurs `*`, négation `[^x]`, ancres `$`
