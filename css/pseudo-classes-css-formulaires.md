---
tags: [css, selecteurs, pseudo-classes, formulaires]
---

# Pseudo-classes de formulaire

Ciblent les champs selon leur **état** (activé, coché, valide…), synchronisé en direct avec le DOM, contrairement aux sélecteurs d'attributs.

## Liste

| Pseudo-classe | Cible |
|---|---|
| `:enabled` / `:disabled` | champ actif / désactivé (`disabled`, ou dans un `fieldset[disabled]`) |
| `:read-only` / `:read-write` | non modifiable / modifiable par l'utilisateur (inclut `contenteditable`) |
| `:required` / `:optional` | avec / sans l'attribut `required` |
| `:checked` | checkbox ou radio cochée, `<option>` sélectionnée |
| `:indeterminate` | checkbox avec `el.indeterminate = true`, groupe radio sans choix, `<progress>` sans valeur |
| `:default` | élément par défaut d'un groupe (bouton submit principal, option ou case cochée initialement) |
| `:valid` / `:invalid` | respecte / viole les contraintes (`required`, `type`, `pattern`, `min`…) — aussi sur `<form>` et `<fieldset>` |
| `:user-valid` / `:user-invalid` | idem, mais **seulement après interaction** de l'utilisateur |
| `:in-range` / `:out-of-range` | valeur dans ou hors `min`/`max` |
| `:placeholder-shown` | champ qui **affiche actuellement** son placeholder (donc vide) |
| `:autofill` | champ pré-rempli par le navigateur |
| `:blank` | champ vide (spécification, **non supporté**) |

```css
input:disabled, fieldset:disabled label { opacity: .5; cursor: not-allowed; }

/* Erreurs affichées seulement après saisie (et pas au chargement de la page) */
input:user-invalid { border-color: crimson; }
input:user-invalid + .error { display: block; }

form:invalid button[type="submit"] { opacity: .6; }
fieldset:has(:user-invalid) legend { color: crimson; }

/* Label flottant */
.field input:not(:placeholder-shown) + label,
.field input:focus + label { translate: 0 -1.2em; font-size: .8em; }

/* Checkbox custom */
input[type="checkbox"]:checked + .toggle { background: var(--primary); }
input:indeterminate + .toggle::after { content: "–"; }

input:autofill { box-shadow: 0 0 0 50px white inset; }  /* neutralise le fond jaune */
label:has(+ input:required)::after { content: " *"; color: crimson; }
```

## Détails

- **`:invalid` vs `:user-invalid`** : `:invalid` est vrai **dès le chargement** (un champ `required` vide est invalide), d'où des formulaires rouges avant toute saisie. `:user-invalid` (supporté partout depuis fin 2023) règle ce problème.
- `:checked` suit la **propriété** (clic, JS), alors que `[checked]` ne suit que l'attribut initial.
- Les validations custom (`setCustomValidity()`) sont prises en compte par `:invalid`.
- Angular ajoute ses propres classes (`.ng-invalid`, `.ng-touched`, `.ng-dirty`), qui s'utilisent en complément : `input.ng-invalid.ng-touched`.

## Voir aussi

- [[pseudo-classes-css-interaction]]
- [[pseudo-classes-css-logiques]]
- [[formulaires-angular]]
