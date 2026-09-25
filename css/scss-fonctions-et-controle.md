---
tags: [css, scss, sass]
---

# Fonctions, maps et directives de contrôle SCSS

Sass est un vrai petit langage : **fonctions**, **maps** (dictionnaires), **listes**, `@if`, `@each`, `@for`, `@while`.

## Syntaxe / Exemple

```scss
@use 'sass:math';
@use 'sass:map';
@use 'sass:color';

// Fonction personnalisée : renvoie une valeur
@function rem($px, $base: 16px) {
  @return math.div($px, $base) * 1rem;   // math.div, et non `/` (déprécié)
}

// Map
$colors: (primary: #2f6f4e, danger: crimson, muted: #888);

// @each sur une map → génère des classes utilitaires
@each $name, $value in $colors {
  .text-#{$name} { color: $value; }
  .bg-#{$name}-light { background: color.scale($value, $lightness: 80%); }
}

// @for → échelle d'espacements
@for $i from 1 through 4 {
  .mt-#{$i} { margin-top: $i * 4px; }
}

// @if / @else
@mixin theme($mode) {
  @if $mode == dark { background: #111; color: #eee; }
  @else { background: #fff; color: #111; }
}

h1 { font-size: rem(32px); }                // → 2rem
.x { color: map.get($colors, danger); }
```

## Détails

- **Modules intégrés** (à charger via `@use`) : `sass:math`, `sass:color`, `sass:map`, `sass:list`, `sass:string`, `sass:meta`, `sass:selector`.
- Les fonctions globales historiques (`lighten()`, `darken()`, `map-get()`, `/` pour diviser) sont **dépréciées**. On utilise `color.scale` / `color.adjust`, `map.get`, `math.div`.
- `@debug`, `@warn` et `@error` affichent des messages ou bloquent la compilation (validation d'arguments).
- Tout est évalué **au build**. Pour du dynamique au runtime, on utilise les custom properties CSS ou `color-mix()`.

## Voir aussi

- [[scss-mixins]]
- [[scss-variables]]
- [[scss-modules-use-forward]]
