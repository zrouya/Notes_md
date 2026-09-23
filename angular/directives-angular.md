---
tags: [angular, directives, fondamentaux]
---

# Directives Angular

Les directives sont l'un des principaux éléments constitutifs d'[[angular-overview|Angular]]. Elles sont utilisées pour ajouter du comportement à un élément DOM existant. Angular en distingue trois types.

## Directives de composants

Ce sont en fait des composants : un composant est un type spécial de directive avec un template HTML. Chaque composant créé est, implicitement, une directive.

## Directives structurelles

Elles modifient la structure du DOM en ajoutant ou en supprimant des éléments. Exemples fournis par Angular : [[angular-ngif|`*ngIf`]] (affiche ou masque un élément selon une condition) et [[angular-ngfor|`*ngFor`]] (itère sur un tableau pour afficher une liste d'éléments).

```html
<div *ngIf="condition">Contenu</div>
```

```html
<li *ngFor="let item of items">{{item}}</li>
```

## Directives d'attributs

Elles modifient l'apparence ou le comportement d'un élément DOM existant. Exemple : `ngClass`, qui ajoute ou supprime des classes CSS selon une condition.

```html
<div [ngClass]="{'active': isActive}">Contenu</div>
```

## Créer une directive personnalisée

On utilise le décorateur `@Directive` et on spécifie un sélecteur :

```js
import { Directive, ElementRef } from '@angular/core';

@Directive({  
    selector: '[appHighlight]'
 })
 
export class HighlightDirective {
    constructor(el: ElementRef) {
         el.nativeElement.style.backgroundColor = 'yellow';
    }
 }
```

Utilisation dans un template :

```html
<p appHighlight>Texte en surbrillance</p>
```

Les composants étant des directives avec un template, tout ce qui s'applique aux directives s'applique également aux composants.

## Voir aussi

- [[angular-ngif]]
- [[angular-ngfor]]
- [[composants-angular]]
