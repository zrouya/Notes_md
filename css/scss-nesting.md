---
tags: [css, scss, sass]
---

# Imbrication SCSS et sélecteur parent `&`

Les règles s'imbriquent comme le HTML. `&` désigne le **sélecteur parent** complet.

## Syntaxe / Exemple

```scss
.nav {
  display: flex;

  a { color: inherit; }                 // → .nav a
  > li { margin: 0 8px; }               // → .nav > li

  &:hover { opacity: .9; }              // → .nav:hover
  &.is-open { height: auto; }           // → .nav.is-open
  .dark-theme & { background: #111; }   // → .dark-theme .nav

  // Suffixe (convention BEM)
  &__item { padding: 4px; }             // → .nav__item
  &--compact { gap: 2px; }              // → .nav--compact

  // Media query imbriquée : remonte au niveau racine à la compilation
  @media (max-width: 600px) { flex-direction: column; }

  // Propriétés imbriquées (namespace)
  font: { family: sans-serif; size: 14px; }  // → font-family, font-size
}
```

## Détails

- **Ne pas dépasser 3 niveaux** : sinon, sélecteurs trop spécifiques, CSS lourd et difficile à surcharger.
- `&` sans espace = même élément (`&:hover`, `&.active`), avec espace = descendant.
- `&__x` (concaténation) n'existe **qu'en Sass**. Le CSS natif ne sait pas concaténer.
- **Imbrication CSS native** (supportée par les navigateurs modernes depuis 2023) : syntaxe proche, mais sans concaténation `&__item` et avec quelques différences de spécificité (`:is()`).
- Dans un composant Angular, les styles étant déjà isolés, on a moins besoin d'imbriquer profondément (cf. [[angular-styles-scss]]).

## Voir aussi

- [[scss-overview]]
- [[scss-mixins]]
