
Un composant angular va rendre son template en y gérant les données dynamiques via plusieurs moyens : 
- String interpolation
- Property binding
- Attribute binding
- Event binding
- Class binding

Exemple : 

```typescript
import { Component } from '@angular/core';

interface User {
	id: number;
	name: string;
	avatarUrl: string;
}
@Component({
	templateUrl: 'user/user.component.html'
})
export class UserComponent {
	currentUser: User = { // Propriété currentUser de la classe component
		id: 1,
		name: 'Bob',
		avatar: 'BobThumb.png'
	};
	isSelected: boolean;
	// get accessors can be used for computed values : 
	get imagePath() {
		return 'assets/Users/' + this.currentUser.avatar;
	}

	onSelectUser() {
		isSelected = true;
	}	
}
```

Template user.component.html : 
```html
<div class='user-ui' [class.selected]="isSelected"> <!-- Class binding : 'selected' class is added to the element depending on the binded value -->
	<button (click)="onSelectUser()"> <!-- Event binding -->
		<!-- Property binding, possibly using accessors -->
		<img [src]="imagePath" [alt]="currentUser.name"/> 
		<span class='user-name'>{{ currentUser.name }} </span> <!-- String interpolation. For inline data -->
	</button>
</div>
```

En cas de modification des données du composant, Angular [[State managment Angular|met à jour]] le DOM.