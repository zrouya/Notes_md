
L'injection de dépendance en Angular s'effectue grâce au [[Décorateurs en TypeScript|décorateur]] ``@Injectable`` (à importer depuis ``@angular/core``).

Classe [[Services Angular|service]] : 
```typescript
import [ Injectable ] from '@angular/core';

@Injectable ({ providedIn: 'root'})
export class SomeService {

}
```

Classe consommant le service :
```typescript
class SomeClass {
	private someService: SomeService
	
	constructor(someInjectedService: SomeService) {
		someService = someInjectedService;
	}
}

// Version moins verbeuse :
class SomeClass {
	constructor(private someService: SomeService)
}

// Ou encore :
import [ inject ] from '@angular/core'
class SomeClass {
	private someService = inject(SomeService);
}
```
