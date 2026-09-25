---
tags: [index, css, scss]
---

# CSS / SCSS — Map of Content

## Sélecteurs

- [[selecteurs-css-overview]] — Anatomie (simple, composé, complexe, liste) et carte de toutes les catégories
- [[selecteurs-css-simples]] — `*`, type, `.classe`, `#id`, namespaces, échappement
- [[selecteurs-css-attributs]] — `[attr]`, `=`, `~=`, `|=`, `^=`, `$=`, `*=`, flags `i` / `s`
- [[combinateurs-css]] — Descendant, `>`, `+`, `~`, colonne `||`
- [[pseudo-classes-css-interaction]] — `:hover`, `:focus-visible`, `:focus-within`, liens, `:target`, `:scope`
- [[pseudo-classes-css-structurelles]] — `:nth-child(An+B of S)`, `-of-type`, `:empty`, quantity queries
- [[pseudo-classes-css-formulaires]] — `:checked`, `:user-invalid`, `:placeholder-shown`, `:autofill`…
- [[pseudo-classes-css-logiques]] — `:is()`, `:where()`, `:not()`, `:has()`
- [[pseudo-classes-css-autres]] — `:lang()`, `:dir()`, `:modal`, `:popover-open`, `:defined`, `:state()`, médias
- [[pseudo-elements-css]] — `::before`/`::after`, `::marker`, `::backdrop`, `::highlight()`, View Transitions
- [[selecteurs-css-shadow-dom]] — `:host`, `:host-context()`, `::slotted()`, `::part()`
- [[specificite-css]] — Calcul (A,B,C), exemples, ordre complet de la cascade, `@layer`
- [[selecteurs-css-bonnes-pratiques]] — Évaluation droite → gauche, conventions, `querySelector` / `matches` / `closest`

## SCSS / Sass

- [[scss-overview]] — Préprocesseur CSS : principe, syntaxes, Dart Sass, features principales
- [[scss-variables]] — Variables `$`, `!default`, portée, comparaison avec les custom properties CSS
- [[scss-nesting]] — Imbrication, sélecteur parent `&`, BEM, media queries imbriquées
- [[scss-mixins]] — `@mixin` / `@include`, paramètres, `@content`
- [[scss-extend-placeholders]] — `@extend`, placeholders `%`, limites
- [[scss-fonctions-et-controle]] — `@function`, maps, `@each` / `@for` / `@if`, modules `sass:*`
- [[scss-modules-use-forward]] — `@use` / `@forward`, namespaces, remplacement de `@import`

## Voir aussi

- [[angular-styles-scss]] — SCSS dans Angular (encapsulation, `:host`, `angular.json`)
