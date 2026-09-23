---
tags: [angular, directives, ngif]
---

# Angular — ngIf

Directive structurelle permettant de rendre le rendu d'un [[composants-angular|composant]] conditionnel.

## Syntaxe classique (`*ngIf`)

Il faut importer `NgIf` depuis `@angular/common` au sein du composant (option `imports` du décorateur `@Component`) :

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

## Nouvelle syntaxe (Angular 17+)

Depuis la version 17, la syntaxe a été simplifiée :

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

## Voir aussi

- [[directives-angular]]
- [[angular-ngfor]]
- [[composants-angular]]
