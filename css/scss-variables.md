---
tags: [css, scss, sass]
---

# Variables SCSS

Valeurs nommées préfixées par `$`, résolues **à la compilation**.

## Syntaxe / Exemple

```scss
$primary: #2f6f4e;
$spacing: 8px;
$font-stack: 'Inter', system-ui, sans-serif;

.btn {
  padding: $spacing ($spacing * 2);      // calculs possibles
  font-family: $font-stack;
  background: $primary;
}

// Valeur par défaut surchargeable par le consommateur du module
$radius: 4px !default;

// Portée : une variable déclarée dans un bloc est locale
.alert {
  $local-color: crimson;                 // n'existe que dans .alert
  color: $local-color;
}
```

## Variables SCSS vs custom properties CSS

```scss
:root { --primary: #{$primary}; }        // interpolation #{} : SCSS → CSS
.btn { color: var(--primary); }
```

| | `$variable` (SCSS) | `--variable` (CSS) |
|---|---|---|
| Résolution | Compilation | Exécution (navigateur) |
| Modifiable au runtime (JS, media query, thème sombre) | Non | Oui |
| Utilisable dans les fonctions Sass (`color.scale`…) | Oui | Non |
| Héritage par la cascade DOM | Non | Oui |

## Détails

- **Bonne pratique** : `$` pour les constantes de design (breakpoints, échelles) et `--` pour ce qui change au runtime (thèmes clair/sombre).
- `!default` n'affecte la variable que si elle n'est pas déjà définie → utilisé avec `@use 'lib' with ($radius: 8px)`.
- `!global` permet de modifier une variable globale depuis un bloc (à éviter).
- `#{}` (**interpolation**) injecte une valeur dans un sélecteur, une propriété ou une chaîne : `.icon-#{$name}`.

## Voir aussi

- [[scss-overview]]
- [[scss-modules-use-forward]]
