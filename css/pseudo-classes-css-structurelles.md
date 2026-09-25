---
tags: [css, selecteurs, pseudo-classes]
---

# Pseudo-classes structurelles (`:nth-child`, `:first-of-type`…)

Ciblent un élément selon sa **position parmi ses frères** ou la structure du document.

## Liste complète

| Pseudo-classe | Cible |
|---|---|
| `:root` | racine du document (`<html>`), lieu des variables globales |
| `:empty` | élément **sans enfant** (ni élément, ni texte, **ni espace**, commentaires tolérés) |
| `:first-child` / `:last-child` | premier / dernier enfant de son parent |
| `:only-child` | enfant unique |
| `:nth-child(An+B)` | n-ième enfant (compté depuis le début) |
| `:nth-last-child(An+B)` | n-ième enfant compté depuis la fin |
| `:nth-child(An+B of S)` | n-ième **parmi les frères qui correspondent à S** |
| `:first-of-type` / `:last-of-type` | premier / dernier **de son type** de balise |
| `:only-of-type` | seul de son type parmi ses frères |
| `:nth-of-type(An+B)` / `:nth-last-of-type(An+B)` | n-ième de son type |

## Formule `An+B`

`n` prend les valeurs 0, 1, 2… et on ne garde que les résultats ≥ 1.

| Formule | Éléments | Usage |
|---|---|---|
| `3` | 3e | position fixe |
| `odd` = `2n+1` | 1, 3, 5… | lignes zébrées |
| `even` = `2n` | 2, 4, 6… | |
| `3n` | 3, 6, 9… | tous les 3 |
| `n+4` | 4, 5, 6… | **à partir** du 4e |
| `-n+3` | 1, 2, 3 | **les 3 premiers** |
| `:nth-child(n+3):nth-child(-n+6)` | 3 à 6 | plage (intersection de deux conditions) |
| `:nth-child(-n+2 of .item)` | 2 premiers `.item` | filtré par sélecteur |

```css
tr:nth-child(even) { background: var(--row-alt); }
li:not(:last-child) { border-bottom: 1px solid #ddd; }   /* séparateurs */
.grid > :nth-child(3n+1) { clear: left; }
tr:nth-child(odd of :not([hidden])) { }   /* zébrage correct malgré les lignes masquées */
p:first-of-type::first-letter { font-size: 2em; }        /* lettrine */
.empty-state:empty { display: none; }

/* "Quantity queries" : styler selon le nombre d'enfants */
li:first-child:nth-last-child(n+5),
li:first-child:nth-last-child(n+5) ~ li { font-size: .9em; }   /* ≥ 5 éléments */
ul:has(> li:nth-child(5)) { }                                   /* équivalent moderne */
```

## Détails

- **`-child` compte tous les frères**, alors que **`-of-type` compte uniquement la même balise** : `p:first-child` ne correspond à rien si un `h2` précède le premier `p`, mais `p:first-of-type` correspond.
- `-of-type` se base sur le **nom de balise**, pas sur la classe : `.item:first-of-type` n'est pas « la première `.item` ». Pour ça, on utilise `:nth-child(1 of .item)`.
- `:empty` échoue si le HTML contient un saut de ligne ou un espace (`<div> </div>`).
- Spécificité : (0,1,0), et `:nth-child(An+B of S)` ajoute la spécificité de S.

## Voir aussi

- [[combinateurs-css]]
- [[pseudo-classes-css-logiques]]
- [[specificite-css]]
