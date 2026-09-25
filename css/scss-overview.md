---
tags: [css, scss, sass]
---

# SCSS / Sass — vue d'ensemble

**Sass** est un préprocesseur CSS : on écrit un langage enrichi, compilé en CSS standard au build. **SCSS** est sa syntaxe principale (accolades + `;`), un sur-ensemble strict de CSS.

## Exemple

```scss
@use 'sass:color';

$primary: #2f6f4e;                        // variable

.card {                                   // imbrication
  border: 1px solid $primary;
  &:hover { background: color.scale($primary, $lightness: 85%); }
  .title { font-weight: 600; }
}
```

Compilé en :

```css
.card { border: 1px solid #2f6f4e; }
.card:hover { background: #e0ede6; }
.card .title { font-weight: 600; }
```

## Détails

- **Deux syntaxes** : `.scss` (type CSS, la plus utilisée) et `.sass` (indentation, sans accolades ni `;`).
- **Tout CSS valide est du SCSS valide** → adoption progressive : renommer `.css` en `.scss` suffit.
- **Implémentation de référence : Dart Sass** (package npm `sass`). LibSass / node-sass sont **dépréciés**.
- La compilation est **statique** : variables, boucles, fonctions disparaissent du CSS final. Rien n'est évalué dans le navigateur.
- Fichiers **partiels** : `_nom.scss` (préfixe `_`) = fichier destiné à être importé, jamais compilé seul.
- Features principales :
  - [[scss-variables]] — valeurs réutilisables
  - [[scss-nesting]] — imbrication et sélecteur parent `&`
  - [[scss-mixins]] — blocs de déclarations paramétrables
  - [[scss-extend-placeholders]] — héritage de règles
  - [[scss-fonctions-et-controle]] — fonctions, maps, `@if` / `@each` / `@for`
  - [[scss-modules-use-forward]] — découpage en modules
- **Concurrence du CSS natif** : custom properties (`--x`), imbrication native, `color-mix()`, `@layer`… couvrent une partie des usages. Sass garde l'avantage pour les mixins, boucles, maps et fonctions de build.

## Voir aussi

- [[angular-styles-scss]]
- [[_index-css]]
