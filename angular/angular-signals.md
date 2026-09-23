---
tags: [angular, signals, state-management]
---

# Angular Signals

À partir de la version 16 d'Angular, la [[state-managment-angular|gestion d'état]] des composants peut être implémentée par des Signals, une feature reposant sur des souscriptions à des événements de mise à jour de données. Les classes de type ViewModel (ou tout autre objet incluant des données consommées par les composants) sont alors wrappées dans un trackable data container, un Signal.

## Exemple

Le code pris en exemple dans [[composants-angular-donnees-dynamiques|Composants Angular - Données dynamiques]] se réécrit ainsi :

```typescript
using { signal, computed } from '@angular/core'

interface User {
	id: number;
	name: string;
	avatarUrl: string;
}
@Component({
	templateUrl: 'user/user.component.html'
})
export class UserComponent {
	currentUser: User = signal({ // Initialisation d'un objet Signal
		id: 1,
		name: 'Bob',
		avatar: 'BobThumb.png'
	});
	
	imagePath = computed(() => { 'assets/Users/' + currentUser().avatar; });

	onSelectUser() {
		// Pour mettre à jour la valeur d'un Signal, on appelle la fonction set
		currentUser.set(new User() { id: 2, name: 'Alice', avatar:'Alice.png'});
	}	
}
```

Template `user.component.html` :
```html
<div class='user-ui'>
	<button (click)="onSelectUser()">
		<!-- Property binding, based on Signal computed value -->
		<img [src]="imagePath()" [alt]="currentUser().name"/> 
		<span class='user-name'>{{ currentUser().name }} </span> <!-- Accessing Signal value -->
	</button>
</div>
```

## Voir aussi

- [[state-managment-angular]]
- [[composants-angular-donnees-dynamiques]]
- [[angular-component-inputs-outputs]] — syntaxe Signals pour les Inputs/Outputs
