---
tags: [css, selecteurs, pseudo-classes, accessibilite]
---

# Pseudo-classes d'interaction, de focus et de lien

Ciblent un élément selon une **action de l'utilisateur** ou l'**état de navigation**. Spécificité (0,1,0).

## Action utilisateur et focus

| Pseudo-classe | Cible |
|---|---|
| `:hover` | élément survolé (et ses ancêtres) |
| `:active` | pendant le clic / l'appui |
| `:focus` | élément qui a le focus (clavier, souris, JS) |
| `:focus-visible` | focus **qui doit être montré** (en pratique : navigation clavier) |
| `:focus-within` | élément dont lui-même **ou un descendant** a le focus |

```css
/* Focus accessible : anneau au clavier seulement */
button:focus { outline: none; }
button:focus-visible { outline: 2px solid var(--focus); outline-offset: 2px; }

.search:focus-within { box-shadow: 0 0 0 2px var(--primary); }  /* le conteneur réagit */

/* Survol seulement sur appareils qui le supportent (pas sur tactile) */
@media (hover: hover) { .card:hover { translate: 0 -2px; } }
```

## Liens et navigation

| Pseudo-classe | Cible |
|---|---|
| `:link` | lien (`a`/`area` avec `href`) **non visité** |
| `:visited` | lien visité (propriétés limitées : couleurs uniquement, pour la vie privée) |
| `:any-link` | tout lien avec `href`, visité ou non |
| `:target` | élément dont l'`id` correspond au **fragment d'URL** (`#section-2`) |
| `:scope` | élément de référence : racine en CSS, élément appelant dans `el.querySelectorAll(':scope > li')`, racine d'un `@scope` |

```css
a:link, a:visited { color: var(--link); }      /* ordre historique : LoVe HAte */
a:hover { text-decoration: underline; }        /* :link :visited :hover :active */
a:active { color: var(--link-active); }

:target { scroll-margin-top: 80px; animation: flash 1s; }  /* ancre ciblée */
```

## Détails

- **Ordre LVHA** : `:link`, `:visited`, `:hover`, `:active` ont la même spécificité, donc la dernière règle gagne, d'où cet ordre.
- **Ne jamais supprimer** l'outline sans le remplacer : c'est une exigence d'accessibilité (WCAG 2.4.7).
- `:focus-visible` suit une heuristique du navigateur : un champ texte cliqué à la souris l'active quand même (car la saisie clavier va suivre).
- `:hover` sur tactile : « collant » après un tap, d'où `@media (hover: hover)`.
- `:visited` : les navigateurs mentent à `getComputedStyle` et n'autorisent que `color`, `background-color`, `border-color`, `outline-color`, `fill`, `stroke`, afin d'empêcher l'espionnage de l'historique.

## Voir aussi

- [[pseudo-classes-css-formulaires]]
- [[pseudo-classes-css-autres]]
- [[specificite-css]]
