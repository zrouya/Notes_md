---
tags: [css, selecteurs]
---

# Combinateurs CSS

Relient deux sélecteurs selon la **relation dans l'arbre DOM**. Ils n'ajoutent aucune spécificité.

## Syntaxe / Exemple

| Combinateur | Nom | `A ? B` cible B quand… |
|---|---|---|
| `A B` (espace) | descendant | B est **n'importe où sous** A |
| `A > B` | enfant | B est un **enfant direct** de A |
| `A + B` | frère adjacent | B **suit immédiatement** A (même parent) |
| `A ~ B` | frères suivants | B vient **après** A (même parent, pas forcément collé) |
| `A \|\| B` | colonne | B est une cellule de la colonne A (**non supporté** par les navigateurs) |

```html
<article>
  <h2>Titre</h2>
  <p>1</p>          <!-- h2 + p, h2 ~ p, article > p, article p -->
  <div><p>2</p></div>   <!-- article p seulement -->
  <p>3</p>          <!-- h2 ~ p, article > p, article p -->
</article>
```

```css
.menu a        { }  /* tous les liens dans .menu, même imbriqués */
.menu > li     { }  /* seulement les li de 1er niveau */
h2 + p         { margin-top: 0; }           /* paragraphe collé au titre */
.stack > * + * { margin-top: 1rem; }        /* "lobotomized owl" : espace entre enfants */
input:checked ~ .panel { display: block; }  /* toggle sans JS */
label + input:invalid { }

/* Enchaînements */
ul > li > a    { }
.form-row > label + input:focus ~ .hint { }
```

## Détails

- La direction est **uniquement vers le bas ou vers l'avant** : il n'y a pas de combinateur « parent » ni « frère précédent ». Pour ça, on utilise **`:has()`** : `li:has(> a.active)` cible le parent, `h2:has(+ p)` le frère précédent (cf. [[pseudo-classes-css-logiques]]).
- **Descendant vs enfant** : `>` évite de styler par erreur des composants imbriqués (menus multi-niveaux, cartes dans des cartes).
- Espaces autour de `>`, `+` et `~` : optionnels (`a>b` ≡ `a > b`).
- **Imbrication** (CSS natif et SCSS) : un combinateur peut ouvrir la règle imbriquée : `.card { > h2 { } + .card { } }` (cf. [[scss-nesting]]).
- Le sélecteur est **évalué de droite à gauche** par le moteur : `.menu a` teste d'abord chaque `a`, puis remonte les ancêtres (cf. [[selecteurs-css-bonnes-pratiques]]).

## Voir aussi

- [[selecteurs-css-overview]]
- [[pseudo-classes-css-structurelles]]
- [[pseudo-classes-css-logiques]]
