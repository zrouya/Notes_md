---
tags: [css, selecteurs, pseudo-classes]
---

# Autres pseudo-classes : langue, état d'éléments, médias

Pseudo-classes plus spécialisées : **langue et direction**, **état d'éléments interactifs** (dialog, popover, plein écran), **custom elements**, **lecture média**.

## Langue et direction

| Pseudo-classe | Cible |
|---|---|
| `:lang(fr)` | élément dont la langue (héritée de `lang`) est `fr` ou `fr-*`. Accepte une liste : `:lang(fr, en)` |
| `:dir(rtl)` / `:dir(ltr)` | direction **calculée** (via `dir`, ou `auto` selon le contenu) |

```css
:lang(fr) q { quotes: "« " " »"; }
:lang(de) q { quotes: "„" "“"; }
.icon-arrow:dir(rtl) { scale: -1 1; }   /* miroir en arabe / hébreu */
```

## État d'éléments et d'API

| Pseudo-classe | Cible |
|---|---|
| `:fullscreen` | élément en plein écran (`el.requestFullscreen()`) |
| `:modal` | élément modal : `dialog.showModal()` ou plein écran |
| `:popover-open` | élément `[popover]` affiché |
| `:open` | `<details>`, `<dialog>`, `<select>` ou `<input>` à picker **ouvert** (récent) |
| `:defined` | élément standard, ou custom element **déjà enregistré** (`customElements.define`) |
| `:state(nom)` | état custom d'un custom element (`this.internals.states.add('nom')`) |
| `:picture-in-picture` | vidéo en mode Picture-in-Picture |

```css
my-widget:not(:defined) { visibility: hidden; }   /* évite le flash avant l'upgrade */
dialog:modal { max-width: 40rem; }
[popover]:popover-open { opacity: 1; }
@starting-style { [popover]:popover-open { opacity: 0; } }  /* animation d'entrée */
details:open summary { font-weight: bold; }       /* sinon details[open] */
video:fullscreen { object-fit: contain; }
```

## Médias (support limité, surtout Safari)

`:playing`, `:paused`, `:seeking`, `:buffering`, `:stalled`, `:muted`, `:volume-locked` sur `<audio>` et `<video>`.

## Détails

- `:lang()` est plus robuste que `[lang|="fr"]` : il prend en compte la langue **héritée** d'un ancêtre, alors que l'attribut ne regarde que l'élément lui-même.
- `:dir()` est plus fiable que `[dir="rtl"]` pour la même raison (héritage et `dir="auto"`).
- Pour les éléments sans pseudo-classe dédiée, on utilise l'attribut : `details[open]`, `dialog[open]`.
- `:host`, `:host()`, `:host-context()` (composants) : cf. [[selecteurs-css-shadow-dom]].

## Voir aussi

- [[pseudo-classes-css-interaction]]
- [[pseudo-elements-css]]
- [[selecteurs-css-shadow-dom]]
