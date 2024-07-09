
Pour rendre le rendu d'un [[Composants Angular|composants]] conditionnel : 

Il faut importer la directive structurelle NgIf  depuis @angular/common au sein du composant (dans l'option Imports du décorateur Component): 
```typescript
import { Component } from '@angular/core';
import { NgFor, NgIf } from '@angular/common';

@Component({
	selector: 'app-root',
	templateUrl: 'app.component.html',
	imports: [ NgIf, NgFor, UserComponent ]
})
export class AppComponent {
	users: User[];
	selectedUser: User;

	onSelectUser(user: User) {
		selectedUser = user;
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
	<app-user-detail *ngIf="selectedUser; else fallback" [name]="selectedUser!.name" />
	<ng-template #fallback>
		<p>Select a User</p>
	</ng-template>
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
	@if(selectedUser) {
		<app-user-detail [user]="selectedUser" />
	} @else {
		<p>Select a User</p>
	}
</main>
```