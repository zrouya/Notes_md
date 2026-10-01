---
tags: [angular, composants, data-binding]
---

# Angular — Component Inputs/Outputs

Pour définir un [[composants-angular|composant]] ayant une propriété settable par son parent, on utilise le [[decorateurs-en-typescript|décorateur]] `@Input`. De même, pour qu'un événement du composant soit transmis à son parent, on utilise le décorateur `@Output` avec un objet `EventEmitter`.

## Syntaxe classique (décorateurs)

```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

interface User {
	id: number;
	name: string;
	avatarUrl: string;
}
@Component({
	selector: 'user-card'
	templateUrl: 'user/user.component.html'
})
export class UserComponent {
	@Input({ required: true}) currentUser!: User;
	@Output() select = new EventEmitter<User>();
	
	get imagePath() {
		return 'assets/Users/' + this.currentUser.avatar;
	}

	onSelectUser() {
		// On lève un évenement de l'EventEmitter, éventuellement avec un argument
		this.select.emit(this.currentUser);
	}	
}
```

Template du composant parent :
```html
<div class='user-ui'>
	<user-card [currentUser]="model.selectedUser"
			   (select)="parentComponentDelegate($event)"/> <!-- $event is used to pass the EventEmitter argument to the parent -->
</div>
```

## Syntaxe Signals (Angular 16+)

Depuis la version 16 d'Angular, avec l'arrivée des [[angular-signals|Signals]], la syntaxe diffère (ainsi que l'implémentation technique) :

```typescript
import { Component, input, output, model, computed } from '@angular/core';

interface User {
	id: number;
	name: string;
	avatarUrl: string;
}
@Component({
	selector: 'user-card',
	templateUrl: 'user/user.component.html'
})
export class UserComponent {
	currentUser = input.required<User>();   // input obligatoire
	taille = input(64);                     // input avec valeur par défaut
	select = output<User>();
	favori = model(false);                  // two-way binding : [(favori)]="..."

	// un input étant un signal, on en dérive directement un computed (plus besoin de ngOnChanges)
	imagePath = computed(() => 'assets/Users/' + this.currentUser().avatarUrl);

	onSelectUser() {
		this.select.emit(this.currentUser());
	}

	toggleFavori() {
		this.favori.update(f => !f);   // notifie aussi le parent
	}
}
```

| Décorateur | Équivalent signal |
|---|---|
| `@Input() x!: T` (required) | `x = input.required<T>()` |
| `@Input() x = 10` | `x = input(10)` |
| `@Output() e = new EventEmitter<T>()` | `e = output<T>()` |
| `@Input` + `@Output xChange` | `x = model<T>()` |
| `@ViewChild('ref')` | `ref = viewChild<ElementRef>('ref')` |

## Voir aussi

- [[composants-angular]]
- [[decorateurs-en-typescript]]
- [[angular-signals]]
