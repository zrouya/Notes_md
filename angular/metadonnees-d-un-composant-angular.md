---
tags: [angular, composants, decorateurs]
---

# Métadonnées d'un composant Angular

Dans Angular, les métadonnées d'un composant sont fournies via un [[decorateurs-en-typescript|décorateur]] `@Component` attaché à la classe du composant.

## Exemple

```js
import { Component } from '@angular/core'; // Import du décorateur @Component
import { MonModule, MonService, MonAnimation, AutreComposantStandalone } from ..

@Component({
  selector: 'app-mon-composant', // Nom de la balise HTML correspondant à ce composant
  standalone : true, // Depuis Angular 14, permet de définir un composant qui n'appartient pas à un module
  imports : [MonModule, AutreComposantStandalone],
  templateUrl: './mon-composant.component.html',
  styleUrls: ['./mon-composant.component.css'],
  providers: [MonService], // Utilisé pour injecter les services dont le composant a besoin
  animations: [monAnimation]
})
export class MonComposantComponent {
  // ...
}
```

## Voir aussi

- [[composants-angular]]
- [[decorateurs-en-typescript]]
