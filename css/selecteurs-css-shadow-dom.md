---
tags: [css, selecteurs, web-components, angular]
---

# Sélecteurs de composants : `:host`, `::slotted()`, `::part()`

Sélecteurs liés au **Shadow DOM** (web components), pour styler l'hôte depuis l'intérieur, le contenu projeté, ou des parties exposées depuis l'extérieur. Angular les émule avec `ViewEncapsulation.Emulated`.

## Syntaxe / Exemple

```css
/* ---- À l'intérieur du composant (shadow root / styles du composant Angular) ---- */
:host { display: block; contain: content; }        /* l'élément hôte lui-même */
:host(.compact) { padding: 4px; }                  /* hôte avec la classe .compact */
:host([disabled]) { opacity: .5; pointer-events: none; }
:host(:hover) .toolbar { visibility: visible; }
:host-context(.dark-theme) .panel { background: #111; }  /* un ANCÊTRE a .dark-theme */

::slotted(p) { margin: 0; }                        /* enfants projetés via <slot> */
::slotted([slot="title"]) { font-weight: bold; }

/* ---- À l'extérieur, dans la page qui utilise le composant ---- */
my-card::part(header) { background: var(--primary); }   /* <div part="header"> */
my-card::part(button):hover { }                          /* pseudo-classes permises */
my-card:state(loading) { cursor: progress; }             /* état custom exposé */
```

## Détails

- **`:host`** : spécificité d'une pseudo-classe (0,1,0). **`:host(sel)`** : + spécificité de `sel`. Les styles de la page (extérieur) sur l'hôte **gagnent** sur `:host` à spécificité égale.
- **`:host-context()`** : **abandonné** par la spec et non supporté par Firefox et Safari en Shadow DOM natif. Il fonctionne dans Angular (émulé). Alternative : des custom properties héritées (`--theme-bg`).
- **`::slotted()`** : ne cible que les **enfants directs** projetés, et accepte un seul sélecteur composé (`::slotted(p span)` ✗).
- **`::part()`** : l'auteur du composant expose explicitement des morceaux (`part="header"`). C'est l'API de style officielle. `exportparts` les ré-exporte à travers des composants imbriqués.
- **Custom properties** : elles traversent le Shadow DOM (héritage), ce qui en fait le moyen standard de thémer un composant.
- **Angular** : `:host`, `:host()` et `:host-context()` sont supportés dans les styles de composant. `::ng-deep` (déprécié) perce l'encapsulation émulée (cf. [[angular-styles-scss]]).

## Voir aussi

- [[pseudo-classes-css-autres]]
- [[pseudo-elements-css]]
- [[angular-styles-scss]]
