---
tags: [angular, composants, fondamentaux]
---

# Composants Angular

Les composants sont les blocs de base d'une application [[angular|Angular]]. Un composant contrôle une partie de l'écran appelée vue.

## Anatomie d'un composant

Un composant est composé de trois parties, plus des métadonnées :
- un fichier `.ts` : une classe qui gère les données et les fonctionnalités
- un fichier `.html` : la structure de la vue
- un fichier `.css` (ou `.scss`...) : le style
- des [[metadonnees-d-un-composant-angular|métadonnées]] qui fournissent des informations supplémentaires au composant

## Génération via la CLI

```powershell
ng generate component [path]/[name]

ng g c [path]/[name]
```

## Pour aller plus loin

La classe composant gère les [[composants-angular-donnees-dynamiques|données dynamiques]] affichées dans son template. Pour des composants modulaires et réutilisables, on définit des [[angular-component-inputs-outputs|Inputs/Outputs]].

## Voir aussi

- [[metadonnees-d-un-composant-angular]]
- [[composants-angular-donnees-dynamiques]]
- [[angular-component-inputs-outputs]]
