---
tags: [css, selecteurs, pseudo-classes]
---

# Pseudo-classes logiques : `:is()`, `:where()`, `:not()`, `:has()`

Prennent une **liste de sélecteurs** en argument pour factoriser (`:is`, `:where`), exclure (`:not`) ou tester les descendants et frères (`:has`).

## Syntaxe / Exemple

```css
/* :is() — OU factorisé ; spécificité = celle de l'argument le plus spécifique */
:is(h1, h2, h3):hover { color: var(--primary); }
:is(article, aside) :is(h1, h2) { margin-top: 0; }   /* 4 combinaisons en une ligne */

/* :where() — identique à :is() mais spécificité ZÉRO → facile à surcharger */
:where(ul, ol)[role="list"] { list-style: none; padding: 0; }   /* reset / lib */
:where(.btn) { padding: .5rem 1rem; }   /* .btn-custom le surcharge sans effort */

/* :not() — négation, accepte une liste (= ni l'un ni l'autre) */
a:not([href]) { cursor: default; }
li:not(:first-child, .separator) { margin-top: .5rem; }
input:not([type="checkbox"], [type="radio"]) { width: 100%; }

/* :has() — "sélecteur parent / relationnel" : l'élément contient ou précède… */
.card:has(img) { display: grid; grid-template-rows: auto 1fr; }
li:has(> a.active) { background: #eef; }        /* parent d'un lien actif */
h2:has(+ p) { margin-bottom: .25rem; }          /* h2 suivi directement d'un p */
form:has(:user-invalid) .submit { opacity: .5; }
body:has(dialog[open]) { overflow: hidden; }    /* bloque le scroll quand modale ouverte */
.grid:has(> :nth-child(4)) { grid-template-columns: repeat(2, 1fr); } /* ≥ 4 enfants */
:root:has(#dark-mode:checked) { color-scheme: dark; }  /* thème sans JS */
```

## Tableau récapitulatif

| | Rôle | Spécificité | Liste tolérante (*forgiving*) |
|---|---|---|---|
| `:is()` | correspond à l'un des sélecteurs | max des arguments | Oui |
| `:where()` | idem | **0** | Oui |
| `:not()` | ne correspond à aucun | max des arguments | Non |
| `:has()` | a un descendant ou frère (sélecteur relatif) correspondant | max des arguments | Non |

## Détails

- **Forgiving** : dans `:is()` / `:where()`, un sélecteur invalide ou non supporté est ignoré sans invalider la règle. Ailleurs, une seule erreur invalide toute la liste.
- **Piège de `:is()`** : `:is(#id, p)` donne la spécificité d'un id **même pour les `p`**.
- **`:not()`** : `div:not(.a):not(.b)` ≡ `div:not(.a, .b)`. `:not(*)` ne correspond à rien. Un `:not()` ne cible pas les ancêtres : `:not(.dark) p` correspond presque toujours (via `html` ou `body`).
- **`:has()`** (supporté partout depuis décembre 2023) : argument = **sélecteur relatif** (`> x`, `+ x`, `~ x`, ou `x` pour un descendant). Pas de `:has()` imbriqué ni de pseudo-élément dedans. Il peut coûter cher sur de gros DOM s'il est ancré sur `body` ou `:root`.
- Test de support : `@supports selector(:has(a)) { … }`.

## Voir aussi

- [[specificite-css]]
- [[combinateurs-css]]
- [[selecteurs-css-overview]]
