Les directives sont l'un des principaux éléments constitutifs d'[[Angular]]. Elles sont utilisées pour **ajouter du comportement à un élément DOM existant**.

Angular a trois types de directives :

1. **Composant Directives** :
    
    - Ce sont en fait des composants, qui sont des directives avec un template. Un composant est un type spécial de directive avec un template HTML. Chaque composant que vous créez, implicitement, est une directive.
    
2. **Directives Structurelles** :
    
    - Ces directives modifient la structure du DOM en ajoutant ou en supprimant des éléments du DOM. Par exemple, les directives `*ngIf` et `*ngFor` sont des directives structurelles fournies par Angular. 
    - `*ngIf` est utilisé pour afficher ou masquer un élément en fonction d'une condition. Par exemple :
    ```html
    <div *ngIf="condition">Contenu</div>
```
    
    Le contenu de la div sera affiché seulement si la `condition` est vraie.
    
    - `*ngFor` est utilisé pour itérer sur un tableau et afficher une liste d'éléments. Par exemple :
    ```html
    <li *ngFor="let item of items">{{item}}</li>
```
    
    
    Cela va générer une liste d'éléments li en fonction des éléments du tableau `items`.
    
3. **Directives d'Attributs** :
    
    - Ces directives modifient l'apparence ou le comportement d'un élément DOM existant. Par exemple, la directive `ngClass` est une directive d'attribut fournie par Angular.
    - `ngClass` est utilisé pour ajouter ou supprimer des classes CSS à un élément en fonction d'une condition. Par exemple :
	```html
	`<div [ngClass]="{'active': isActive}">Contenu</div>`
```
    
    La classe `active` sera ajoutée à la div si la variable `isActive` est vraie.
    

Créer une Directive Personnalisée :

- Pour créer une directive personnalisée, vous pouvez utiliser le décorateur `@Directive` et spécifier un sélecteur. Par exemple :

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

- Dans cet exemple, une nouvelle directive `appHighlight` est créée qui modifie la couleur d'arrière-plan de l'élément auquel elle est attachée en jaune. Vous pouvez utiliser cette directive dans un template HTML comme suit :

``` html
<p appHighlight>Texte en surbrillance</p>
```

- Ceci rendra le texte du paragraphe en arrière-plan jaune.

Notez que les composants sont des directives avec un template, donc tout ce qui s'applique aux directives s'applique également aux composants.