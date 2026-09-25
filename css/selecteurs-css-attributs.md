---
tags: [css, selecteurs]
---

# Sélecteurs d'attributs CSS

Ciblent un élément selon la **présence** ou la **valeur** d'un attribut. Spécificité d'une classe (0,1,0).

## Syntaxe / Exemple

| Sélecteur | Correspond si l'attribut… | Exemple ciblé |
|---|---|---|
| `[attr]` | existe (quelle que soit sa valeur) | `[disabled]` |
| `[attr="v"]` | vaut exactement `v` | `[type="email"]` |
| `[attr~="v"]` | contient le **mot** `v` (liste séparée par espaces) | `[rel~="noopener"]` sur `rel="noopener noreferrer"` |
| `[attr\|="v"]` | vaut `v` **ou** commence par `v-` | `[lang\|="en"]` sur `en`, `en-US` |
| `[attr^="v"]` | **commence** par `v` | `[href^="https://"]` |
| `[attr$="v"]` | **finit** par `v` | `[href$=".pdf"]` |
| `[attr*="v"]` | **contient** `v` (sous-chaîne) | `[class*="col-"]` |
| `[attr="v" i]` | comparaison **insensible à la casse** | `[type="SUBMIT" i]` |
| `[attr="v" s]` | comparaison **sensible à la casse** (forcée) | |

```css
a[href^="http"]:not([href*="mondomaine.fr"])::after { content: " ↗"; } /* liens externes */
a[href$=".pdf" i]::before { content: "📄 "; }
a[target="_blank"]:not([rel~="noopener"]) { outline: 2px solid red; }  /* lint visuel */

[data-state="open"]  { display: block; }       /* états pilotés en JS / frameworks */
[aria-expanded="true"] > .chevron { rotate: 180deg; }
[aria-current="page"] { font-weight: bold; }   /* lien actif accessible */
[hidden] { display: none !important; }

input[type="checkbox" i], input:not([type]) { }  /* :not([type]) = input texte par défaut */
```

## Détails

- Les **guillemets** sont optionnels si la valeur est un identifiant valide (`[type=email]`), mais obligatoires sinon (espaces, `/`, `:`, chiffre en tête).
- `[attr=""]` cible un attribut **présent mais vide**. `[attr^=""]`, `[attr$=""]` et `[attr*=""]` ne ciblent **jamais** rien.
- Valeur **`[class="a"]`** : comparaison sur la chaîne entière, donc `class="a b"` ne correspond pas. Pour les classes, on utilise `.a` ou `[class~="a"]`.
- Le sélecteur se base sur l'**attribut HTML**, pas sur la propriété DOM : `input[value="x"]` ne suit pas la saisie de l'utilisateur (seulement la valeur initiale).
- Styler via `aria-*` renforce l'accessibilité : le style suit l'état réellement exposé aux lecteurs d'écran.
- En HTML, les valeurs de certains attributs (`type`, `lang`, `dir`…) sont comparées sans tenir compte de la casse, par compatibilité.

## Voir aussi

- [[selecteurs-css-overview]]
- [[selecteurs-css-simples]]
- [[pseudo-classes-css-logiques]]
