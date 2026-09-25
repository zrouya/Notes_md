---
tags: [css, scss, sass]
---

# `@extend` et placeholders `%` en SCSS

`@extend` fait **hériter** un sélecteur des règles d'un autre en **fusionnant les sélecteurs**, sans dupliquer les déclarations.

## Syntaxe / Exemple

```scss
// Placeholder : jamais émis seul dans le CSS
%message-base {
  padding: 8px 12px;
  border-radius: 4px;
  border: 1px solid;
}

.success { @extend %message-base; color: green; }
.error   { @extend %message-base; color: crimson; }
```

Compilé en :

```css
.success, .error { padding: 8px 12px; border-radius: 4px; border: 1px solid; }
.success { color: green; }
.error { color: crimson; }
```

## Détails

- `%placeholder` = sélecteur « silencieux », **uniquement** destiné à être étendu (pas de classe inutile dans le CSS final).
- On peut étendre une vraie classe (`@extend .btn;`), mais **toutes** les règles contenant `.btn` sont alors réécrites, y compris `.card .btn`… d'où des effets de bord.
- **Limites** :
  - pas d'`@extend` à travers un `@media` vers un sélecteur défini hors de ce média ;
  - avec `@use`, on ne peut étendre que ce qui est dans les modules chargés en amont ;
  - dans Angular, chaque composant est compilé séparément : l'`@extend` ne traverse pas les composants.
- **Recommandation actuelle** : préférer les [[scss-mixins]] (plus prévisibles). Le CSS gzippé absorbe très bien la duplication.

## Voir aussi

- [[scss-mixins]]
- [[scss-overview]]
