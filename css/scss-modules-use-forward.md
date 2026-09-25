---
tags: [css, scss, sass]
---

# Modules SCSS : `@use` et `@forward`

Système de modules de Sass : chaque fichier a son **espace de noms**. Il remplace `@import`, **déprécié** (Dart Sass 1.80, suppression prévue en 3.0).

## Syntaxe / Exemple

```text
styles/
├── _variables.scss
├── _mixins.scss
└── _index.scss        ← point d'entrée du dossier
```

```scss
// styles/_variables.scss
$primary: #2f6f4e !default;
$private-thing: 1px;   // - ou _ en préfixe ($-x) = privé au module

// styles/_index.scss : ré-exporte les modules
@forward 'variables';
@forward 'mixins';

// composant.scss
@use 'styles' as s;                     // charge styles/_index.scss
.btn { color: s.$primary; @include s.flex-center; }

@use 'styles/variables' as *;           // sans namespace (à utiliser avec parcimonie)
@use 'styles/variables' with ($primary: navy);  // configure les !default
```

## Détails

- **Namespace par défaut** = nom du fichier (`@use 'variables'` → `variables.$primary`). `as x` le renomme, `as *` l'aplatit.
- Un module est **chargé une seule fois** par compilation, même s'il est importé plusieurs fois (contrairement à `@import`, qui recopiait).
- `@use` doit être placé **en tête de fichier**.
- `@forward` sert à construire une **API publique** (un seul point d'entrée). Il accepte `show` / `hide` et `as prefix-*`.
- Les membres commençant par `-` ou `_` sont **privés**.
- **Différences avec `@import`** : plus de variables globales implicites, plus de doublons de CSS et une origine explicite de chaque variable.
- Chemins : relatifs au fichier, ou résolus via les `loadPaths` (dans Angular : `stylePreprocessorOptions.includePaths`, cf. [[angular-styles-scss]]).

## Voir aussi

- [[scss-overview]]
- [[scss-variables]]
