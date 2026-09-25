---
tags: [css, scss, sass]
---

# Mixins SCSS (`@mixin` / `@include`)

Bloc de déclarations **réutilisable et paramétrable**, recopié à chaque `@include`.

## Syntaxe / Exemple

```scss
// Mixin sans paramètre
@mixin truncate {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

// Paramètres, valeurs par défaut
@mixin flex-center($direction: row, $gap: 0) {
  display: flex;
  flex-direction: $direction;
  align-items: center;
  justify-content: center;
  gap: $gap;
}

// @content : le mixin reçoit un bloc de styles
$breakpoints: (mobile: 600px, tablet: 960px);

@mixin up-to($bp) {
  @media (max-width: map-get($breakpoints, $bp)) { @content; }
}

.label   { @include truncate; }
.toolbar { @include flex-center($gap: 8px); }        // argument nommé
.sidebar {
  width: 280px;
  @include up-to(mobile) { display: none; }
}
```

## Détails

- Arguments **positionnels** ou **nommés** (`$gap: 8px`), variadiques (`$args...`).
- `@content` permet d'écrire des mixins « enveloppes » (media queries, `:hover` conditionnel, thèmes).
- **Mixin vs `@extend`** : le mixin **duplique** les déclarations (CSS plus gros, mais prévisible), alors que `@extend` **regroupe les sélecteurs** (cf. [[scss-extend-placeholders]]).
- **Mixin vs fonction** : un mixin produit des **déclarations**, une `@function` renvoie une **valeur** (cf. [[scss-fonctions-et-controle]]).
- Usage typique : breakpoints responsives, reset de bouton, focus visible, thèmes Angular Material (`@include mat.button-theme(...)`).

## Voir aussi

- [[scss-overview]]
- [[scss-extend-placeholders]]
- [[scss-fonctions-et-controle]]
