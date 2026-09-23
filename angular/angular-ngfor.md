---
tags: [angular, directives, ngfor]
---

# Angular — ngFor

Directive structurelle permettant de générer une liste de [[composants-angular|composants]] dynamiquement, en itérant sur un tableau.

## Syntaxe classique (`*ngFor`)

Il faut importer `NgFor` depuis `@angular/common` au sein du composant (option `imports` du décorateur `@Component`) :

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

```html
<main class="users-list">
	<ul>
		<li *ngFor="let user of users">
			<app-user [user]="user" (select)="onSelectUser($event)" />
		</li>
	</ul>
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
</main>
```

## Voir aussi

- [[directives-angular]]
- [[angular-ngif]]
- [[composants-angular]]
