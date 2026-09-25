---
tags: [css, selecteurs]
---

# Sélecteurs CSS simples : universel, type, classe, id

Les briques de base : on cible par **nom d'élément**, **classe**, **identifiant**, ou tous les éléments avec `*`.

## Syntaxe / Exemple

```css
*            { box-sizing: border-box; }   /* universel : tous les éléments */
p            { line-height: 1.5; }          /* type (balise) */
.card        { padding: 1rem; }             /* classe */
#main-header { position: sticky; }          /* identifiant */

/* Composés : ET sur le même élément (type toujours en premier) */
button.primary        { }   /* <button class="primary"> */
.btn.btn--large       { }   /* les deux classes */
input#email.invalid   { }

/* Universel implicite : `.x` équivaut à `*.x` */
* > p  { }                  /* tout p enfant direct de n'importe quel élément */

/* Namespaces (SVG / MathML dans du XML, rarement utile en HTML) */
@namespace svg url(http://www.w3.org/2000/svg);
svg|a  { }                  /* <a> dans le namespace SVG uniquement */
*|a    { }                  /* <a> de tous les namespaces */
|a     { }                  /* <a> sans namespace */
```

## Détails

- **Classe** `.x` : correspond si `x` figure parmi les mots de l'attribut `class` (séparés par des espaces). Sensible à la casse.
- **Id** `#x` : unique dans le document en théorie. Si le HTML est invalide et contient des doublons, **tous** les éléments sont quand même ciblés.
- **Échappement** : un identifiant qui commence par un chiffre ou contient des caractères spéciaux doit être échappé : `#\31 23` pour `id="123"`, `.md\:flex` pour `class="md:flex"` (Tailwind). En JS, on utilise `CSS.escape('md:flex')`.
- **Spécificité** : id (1,0,0), classe (0,1,0), type (0,0,1), `*` (0,0,0) → [[specificite-css]].
- **Bonne pratique** : styler par **classes**. On évite les ids en CSS (spécificité trop forte, difficile à surcharger) et les sélecteurs de type trop larges hors reset.
- Le `*` n'est pas lent en soi. `* { box-sizing: border-box }` est un reset standard (souvent avec `*::before, *::after`).

## Voir aussi

- [[selecteurs-css-overview]]
- [[selecteurs-css-attributs]]
- [[specificite-css]]
