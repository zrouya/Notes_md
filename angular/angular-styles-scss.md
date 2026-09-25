---
tags: [angular, scss, css]
---

# Styles et SCSS dans Angular

Angular compile nativement le SCSS (Dart Sass intégré au builder). Chaque composant a ses styles **isolés** (encapsulation), en plus d'une feuille globale.

## Syntaxe / Exemple

```bash
ng new mon-app --style=scss        # choix du préprocesseur à la création
```

```ts
@Component({
  selector: 'app-envelope-card',
  templateUrl: './envelope-card.html',
  styleUrl: './envelope-card.scss',            // styles propres au composant
  // encapsulation: ViewEncapsulation.Emulated (défaut) | None | ShadowDom
})
```

```scss
// envelope-card.scss
@use 'variables' as v;               // résolu via includePaths

:host { display: block; }            // l'élément hôte <app-envelope-card>
:host(.compact) { padding: 4px; }    // hôte avec une classe
.balance.negative { color: v.$danger; }
```

```jsonc
// angular.json → architect.build.options
"styles": ["src/styles.scss"],                          // styles globaux
"inlineStyleLanguage": "scss",                          // styles: [`...`] inline en SCSS
"stylePreprocessorOptions": { "includePaths": ["src/styles"] }
```

## Détails

- **Encapsulation `Emulated`** (défaut) : Angular ajoute des attributs (`_ngcontent-xxx`) aux éléments et réécrit les sélecteurs, donc les styles ne fuient ni vers l'extérieur ni vers les composants enfants.
- `None` : les styles deviennent globaux. `ShadowDom` : vrai Shadow DOM natif.
- `::ng-deep` perce l'encapsulation (styler un enfant ou une lib). Il est **déprécié** mais toujours supporté : préférer des custom properties CSS exposées par l'enfant.
- `src/styles.scss` : reset, typographie, thème, classes utilitaires globales.
- Chaque fichier de composant est compilé **séparément** : les variables et mixins ne sont pas partagés automatiquement, il faut un `@use` explicite dans chaque fichier (sans coût : un `@use` de variables et mixins ne génère aucun CSS).
- **Budgets** (`angular.json`) : `anyComponentStyle` avertit si un composant dépasse la taille configurée.
- **Angular Material** : thème défini en SCSS (`@use '@angular/material' as mat; @include mat.theme(...)`).

## Voir aussi

- [[scss-overview]]
- [[scss-modules-use-forward]]
- [[composants-angular]]
- [[metadonnees-d-un-composant-angular]]
