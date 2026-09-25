---
tags: [css, selecteurs, cascade]
---

# Spécificité CSS

Poids d'un sélecteur, calculé comme un **triplet (A, B, C)** et comparé de gauche à droite. Il départage les règles en conflit à origine et couche égales.

## Calcul

| Colonne | Compte |
|---|---|
| **A** | ids `#x` |
| **B** | classes `.x`, attributs `[x]`, pseudo-classes `:hover` |
| **C** | types `div`, pseudo-éléments `::before` |
| 0 | `*`, combinateurs, `:where()` |

| Sélecteur | Spécificité |
|---|---|
| `li` | (0,0,1) |
| `ul li::marker` | (0,0,3) |
| `.nav a:hover` | (0,2,1) |
| `a[href^="http"]` | (0,1,1) |
| `#main .card > p` | (1,1,1) |
| `:is(#a, .b) p` | (1,0,1) → max de l'argument, même pour `.b` |
| `:where(#a, .b) p` | (0,0,1) |
| `li:not(.done)` | (0,1,1) |
| `.list:has(> li.active)` | (0,3,0) |
| `li:nth-child(2 of .x)` | (0,2,1) |
| `.card { &:hover { } }` (imbrication) | `&` = `:is(.card)` → (0,2,0) |

```css
#nav a       { color: red; }    /* (1,0,1) gagne… */
.nav .link   { color: blue; }   /* (0,2,0) … même avec 20 classes : A l'emporte toujours */
```

## Ordre complet de la cascade (du plus fort au plus faible)

1. **Transitions** en cours
2. `!important` : agent utilisateur > utilisateur > auteur (ordre **inversé**)
3. **Animations** en cours
4. Déclarations normales de l'auteur, puis de l'utilisateur, puis de l'agent utilisateur
5. À origine égale : **style inline** (`style=""`), puis **couches `@layer`** (la dernière déclarée gagne ; hors couche > dans une couche), puis `@scope` (proximité), puis **spécificité**, puis **ordre d'apparition** (la dernière gagne)

## Détails

- Pas de « retenue » entre colonnes : (0,11,0) < (1,0,0).
- `!important` inverse l'ordre des `@layer` (une couche déclarée plus tôt gagne).
- **Baisser la spécificité** : `:where()`, `@layer` (reset / base / composants / utilitaires), classes uniques (BEM).
- **Augmenter** sans id : répéter la classe `.btn.btn` (0,2,0), ou `:is(#id)`.
- DevTools : la spécificité s'affiche au survol d'un sélecteur dans le panneau Styles.
- Une règle **héritée** n'a aucune spécificité face à une règle qui cible directement l'élément, même `*`.

## Voir aussi

- [[selecteurs-css-overview]]
- [[pseudo-classes-css-logiques]]
- [[selecteurs-css-bonnes-pratiques]]
