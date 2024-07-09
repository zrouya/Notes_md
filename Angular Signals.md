
A partir de la version 16 d'Angular, le [[State managment Angular|State Managment]] des composants peut être implémenté par des Signals, une feature reposant sur des souscriptions à des événements de mise à jour de données.
Les classes de type ViewModel (ou tout autre objet incluant des données consommées par les composants Angular) sont wrappées dans un trackable data container, un Signal.

Le code pris en exemple [[Composants Angular - Données dynamiques|ici]] se réécrit alors : 

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

Template user.component.html : 
```html
<div class='user-ui'>
	<button (click)="onSelectUser()">
		<!-- Property binding, based on Signal computed value -->
		<img [src]="imagePath()" [alt]="currentUser().name"/> 
		<span class='user-name'>{{ currentUser().name }} </span> <!-- Accessing Signal value -->
	</button>
</div>
```
