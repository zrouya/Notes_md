
Pour définir un [[Composants Angular|composant]] ayant une propriété settable par son parent, on utilise les [[Décorateurs en TypeScript|décorateurs]] Inputs.
De même, pour qu'un évènement du composant soit transmis par son parent, on utilise le décorateur Output, ainsi qu'un objet EventEmitter :

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

Depuis la version 16 d'Angular, et l'arrivée de la feature [[Angular Signals|Signals]], la syntaxe diffère (ainsi que l'implémentation technique) :


```typescript
import { Component, input, output } from '@angular/core';

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
	currentUser = input.Required<User>();
	select = output<User>();
	
	// get accessors can be used for computed values : 
	get imagePath() {
		return 'assets/Users/' + this.currentUser().avatar;
	}

	onSelectUser() {
		this.select.emit(this.currenUser);
	}	
}
```
