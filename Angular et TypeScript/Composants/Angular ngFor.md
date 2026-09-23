
Pour générer une liste de [[Composants Angular|composants]] dynamiquement : 

Il faut importer la directive structurelle NgFor  depuis @angular/common au sein du composant (dans l'option Imports du décorateur Component): 
```typescript
import { Component } from '@angular/core';
import { NgFor } from '@angular/common';

@Component({
	selector: 'app-root',
	templateUrl: 'app.component.html',
	imports: [ NgFor, UserComponent ]
})
export class AppComponent {
	users: User[];

	onSelectUser() {
		// Do something...
	}	
}
```

Puis, dans le template du composant : 
```html
<main class="users-list">
	<ul>
		<li *ngFor="let user of users">
			<app-user [user]="user" (select)="onSelectUser($event)" />
		</li>
	</ul>
</main>
```


Depuis la version 17 d'Angular, la syntaxe a été simplifiée :
```html
<main class="users-list">
	<ul>
		@for(user of users; track user.id) {
			<li>
				<app-user [user]="user" (select)="onSelectUser($event)" />
			</li>
		}
	</ul>
</main>
```