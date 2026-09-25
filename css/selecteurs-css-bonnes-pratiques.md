---
tags: [css, selecteurs, performance, javascript]
---

# Sélecteurs CSS : bonnes pratiques, performance et usage en JS

Comment le navigateur évalue les sélecteurs, quelles conventions adopter, et comment les réutiliser en JavaScript.

## Évaluation de droite à gauche

```css
.sidebar ul li a { }   /* le moteur part de CHAQUE <a>, puis remonte : li ? ul ? .sidebar ? */
.sidebar-link    { }   /* une seule vérification */
```

- Le **sélecteur de droite** (*key selector*) détermine combien d'éléments sont testés. Il vaut mieux qu'il soit précis (classe).
- Les moteurs modernes (bloom filters, hash par classe ou id) rendent l'impact **négligeable** dans la plupart des cas. On optimise d'abord la **lisibilité**.
- Points de vigilance réels : `:has()` ancré haut (`body:has(...)`) sur un DOM énorme qui change souvent, et `*` en position de sujet combiné à des descendants profonds.

## Conventions

- **Styler par classes**, pas par ids ni par balises (hors reset / typographie de base).
- **Spécificité faible et plate** : 1 à 2 classes. Éviter les chaînes longues (`.page .content .list .item a`).
- **BEM** : `.block__element--modifier` → une classe par élément, pas de dépendance à la structure.
- **Découpler JS et CSS** : `data-*` ou `js-*` pour les hooks JS, classes pour le style.
- États via **attributs ARIA** / `data-state` plutôt que des classes ad hoc : `[aria-expanded="true"]`.
- Ne pas dépendre de la structure fragile (`div > div > span`) : un refactor HTML casse le style.
- **`@layer`** + `:where()` pour les resets et les librairies → facilement surchargeables (cf. [[specificite-css]]).

## Usage en JavaScript

```js
document.querySelector('.card:has(img)')            // premier élément correspondant
document.querySelectorAll('li:nth-child(odd)')      // NodeList statique
list.querySelectorAll(':scope > li')                // enfants directs de `list`
el.matches('.btn:not([disabled])')                  // booléen
el.closest('[data-row-id]')                         // ancêtre le plus proche (ou soi-même)
CSS.supports('selector(:has(a))')                   // détection de support
CSS.escape('md:flex')                               // → "md\\:flex" pour usage dans un sélecteur
```

- `querySelectorAll` renvoie une liste **statique** (pas mise à jour), contrairement à `getElementsByClassName` (liste **live**).
- Les pseudo-éléments ne sont pas sélectionnables. Les pseudo-classes dynamiques (`:hover`, `:focus-visible`) marchent avec `matches()`.
- Un sélecteur invalide lève une `SyntaxError`.

## Voir aussi

- [[specificite-css]]
- [[selecteurs-css-overview]]
- [[pseudo-classes-css-logiques]]
