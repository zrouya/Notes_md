---
tags: [css, selecteurs, pseudo-elements]
---

# Pseudo-éléments CSS (`::before`, `::after`, `::marker`…)

Ciblent une **partie** d'un élément, ou **créent** une boîte virtuelle absente du DOM. Syntaxe `::` (deux-points doubles), spécificité (0,0,1).

## Liste

| Pseudo-élément | Cible |
|---|---|
| `::before` / `::after` | boîte générée en premier / dernier enfant (**requiert `content`**) |
| `::first-line` | première ligne rendue d'un bloc |
| `::first-letter` | première lettre d'un bloc (lettrine) |
| `::marker` | puce / numéro d'un `li` ou `summary` |
| `::placeholder` | texte indicatif d'un champ |
| `::selection` | texte sélectionné par l'utilisateur |
| `::file-selector-button` | bouton de `<input type="file">` |
| `::backdrop` | fond derrière un `dialog` modal, un popover ou le plein écran |
| `::target-text` | texte ciblé par un fragment `#:~:text=…` |
| `::cue` | sous-titres WebVTT d'une vidéo |
| `::highlight(nom)` | plages de texte de la **CSS Custom Highlight API** |
| `::spelling-error` / `::grammar-error` | fautes signalées par le navigateur (support partiel) |
| `::details-content` | contenu repliable d'un `<details>` (récent) |
| `::view-transition`, `::view-transition-group(n)`, `-image-pair(n)`, `-old(n)`, `-new(n)` | arbre de l'**API View Transitions** |
| `::part(nom)` / `::slotted(sel)` | composants web, cf. [[selecteurs-css-shadow-dom]] |

## Exemples

```css
.required::after { content: " *"; color: crimson; }
.external::after { content: " ↗" / ""; }        /* texte alternatif vide pour l'accessibilité */
blockquote::before { content: open-quote; }
.badge::before { content: attr(data-count); }   /* contenu tiré d'un attribut */
.clearfix::after { content: ""; display: table; clear: both; }

li::marker { color: var(--primary); content: "✓ "; }
::selection { background: #ffe58a; }
input::placeholder { color: #999; opacity: 1; }
input[type=file]::file-selector-button { border-radius: 4px; }
dialog::backdrop { background: rgb(0 0 0 / .5); backdrop-filter: blur(2px); }
p::first-line { font-variant: small-caps; }
::view-transition-old(root) { animation: fade-out .2s; }

::highlight(search-hit) { background: yellow; }  /* JS : CSS.highlights.set('search-hit', new Highlight(range)) */
```

## Détails

- Le pseudo-élément doit être **à la fin** du sélecteur : `a:hover::after` ✓, `a::after:hover` ✗ (sauf pseudo-classes autorisées sur certains pseudo-éléments).
- `::before` et `::after` n'existent pas sur les **éléments remplacés ou vides** : `<img>`, `<input>`, `<br>`, `<iframe>`…
- Le contenu de `content` n'est **pas sélectionnable** et reste mal exposé aux lecteurs d'écran : il ne faut pas y mettre d'information essentielle.
- `::first-line`, `::first-letter`, `::marker`, `::selection` et `::placeholder` n'acceptent qu'un **sous-ensemble** de propriétés (typographie, couleurs…).
- La syntaxe à un seul `:` (`:before`, `:after`, `:first-line`, `:first-letter`) est acceptée pour compatibilité CSS2 uniquement.
- Invisible en JS via `querySelector`. Lecture seule avec `getComputedStyle(el, '::before')`.

## Voir aussi

- [[selecteurs-css-overview]]
- [[selecteurs-css-shadow-dom]]
- [[specificite-css]]
