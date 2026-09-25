---
tags: [css, selecteurs]
---

# Sélecteurs CSS — vue d'ensemble

Un sélecteur désigne **les éléments du DOM** auxquels une règle s'applique. On les combine en sélecteurs composés, complexes et listes.

## Anatomie

```css
nav.main > ul li:first-child a[href^="https"]::after { content: "↗"; }
/* └─type+classe─┘ │  │  └─type + pseudo-classe─┘ └attribut─┘ └pseudo-élément
                   │  └ combinateur descendant (espace)
                   └ combinateur enfant (>)                                     */

h1, h2, .title { margin: 0; }     /* liste de sélecteurs (virgule) = OU */
```

## Vocabulaire

- **Sélecteur simple** : un seul critère (`div`, `.x`, `#id`, `[attr]`, `:hover`, `*`).
- **Sélecteur composé** : simples collés, sans espace = **ET** sur le même élément (`a.btn[disabled]:hover`). Le type doit être placé en premier.
- **Sélecteur complexe** : composés reliés par des **combinateurs** (` `, `>`, `+`, `~`).
- **Liste de sélecteurs** : séparés par `,`. Si une partie est invalide, **toute la règle est ignorée** (sauf dans `:is()` / `:where()`, cf. [[pseudo-classes-css-logiques]]).
- **Sujet** du sélecteur : l'élément le plus à droite, qui reçoit les styles.

## Catégories

| Catégorie | Exemples | Note |
|---|---|---|
| Simples | `*` `div` `.class` `#id` | [[selecteurs-css-simples]] |
| Attributs | `[href]` `[type="email"]` `[class^="icon-"]` | [[selecteurs-css-attributs]] |
| Combinateurs | `a b` `a > b` `a + b` `a ~ b` | [[combinateurs-css]] |
| Pseudo-classes d'état / d'interaction | `:hover` `:focus-visible` `:target` | [[pseudo-classes-css-interaction]] |
| Pseudo-classes structurelles | `:nth-child(2n+1)` `:first-of-type` `:empty` | [[pseudo-classes-css-structurelles]] |
| Pseudo-classes de formulaire | `:checked` `:invalid` `:user-invalid` | [[pseudo-classes-css-formulaires]] |
| Pseudo-classes logiques | `:is()` `:where()` `:not()` `:has()` | [[pseudo-classes-css-logiques]] |
| Autres pseudo-classes | `:lang()` `:dir()` `:fullscreen` `:popover-open` | [[pseudo-classes-css-autres]] |
| Pseudo-éléments | `::before` `::marker` `::placeholder` | [[pseudo-elements-css]] |
| Shadow DOM / composants | `:host` `::slotted()` `::part()` | [[selecteurs-css-shadow-dom]] |

## Détails

- La priorité entre règles concurrentes dépend de la **spécificité** : [[specificite-css]].
- Bonnes pratiques, performance et usage en JavaScript : [[selecteurs-css-bonnes-pratiques]].
- Le sélecteur d'imbrication `&` (CSS natif et SCSS) : [[scss-nesting]].
- Les sélecteurs ne sont pas sensibles à la casse pour les noms d'éléments HTML, mais **le sont** pour les classes et les ids.

## Voir aussi

- [[_index-css]]
