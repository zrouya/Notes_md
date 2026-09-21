
Le [[Modules Angular|module]] Angular ``FormsModule`` fournit des directives et des features facilitant la création et la gestion de formulaires (à importer de ``@angular/forms``).

On peut récupérer les données d'un formulaires (ou les mettre à jour) en utilisant le [[Two Way Binding Angular|two way binding]] sur la directive ``NgModel`` d'un élément de formulaire.
La soumission du formulaire se récupère via la directive ``ngSubmit`` (permet à Angular de catcher l'événement de submit sans que le browser n'envoie le formulaire au serveur).

```typescript
import { Component, Output, EventEmitter } from '@angular/core';
import { FormsModule } from '@angular/forms';

interface User {
	id: number;
	name: string;
	password: string;
}
@Component({
	selector: 'new-user',
	templateUrl: 'user/userCreation.component.html',
	imports: [ FormsModule ]
})
export class UserComponent {
	creatingUserName = '';
	creatingUserPassword = '';
	
	@Output() onCreateUser = new EventEmitter<User>();
	 
	onSubmit()
	{
		this.onCreateUser.emit({
			id: someId,
			name: this.creatingUserName,
			password: this.creatingUserPassword
		});
	}
}
```

Template du composant :
```html
<form (ngSubmit)="onCreateUser">
	<label for="name">User name</label>
	<input type="text" id="name" name="name" [(ngModel)]="creatingUserName" />
</form>
```

Le composant parent peut donc récupérer le model correspondant au submit du formulaire (ici User), en se bindant sur l'événement ``onCreateUser`` : ``(onCreateUser)="doSometing($event)"``

**Note** : 
Pour utiliser [[Angular Signals|Signals]] avec le two way binding, la syntaxe du template ne change pas, il suffit d'initialiser les propriétés bindées avec signal() : 

```typescript
import { Component, signal } from '@angular/core';
import { FormsModule } from '@angular/forms';

interface User {
	id: number;
	name: string;
	password: string;
}
@Component({
	templateUrl: 'user/userCreation.component.html'
	imports: [ FormsModule ]
})
export class UserComponent {
	creatingUserName = signal('');
	creatingUserPassword = signal('');
}
```