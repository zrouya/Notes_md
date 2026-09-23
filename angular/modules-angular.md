---
tags: [angular, modules, fondamentaux]
---

# Modules Angular

Les modules [[angular-overview|Angular]] sont des conteneurs pour différentes parties de l'application, comme les [[composants-angular|composants]], les [[directives-angular|directives]] et les [[services-angular|services]]. Le module racine, obligatoire, est conventionnellement appelé `AppModule`.

## Exemple d'AppModule

```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

@NgModule({
	declarations: [
		AppComponent 
	], 
	imports: [
		BrowserModule
	],
	providers: [],
	bootstrap: [AppComponent]
})
export class AppModule { }
```

- `declarations` : liste des composants, directives et pipes appartenant à ce module.
- `imports` : liste des modules dont les composants exportés sont nécessaires dans les templates de ce module.
- `providers` : liste des providers de dépendances instanciés par l'injecteur de ce module.
- `bootstrap` : liste des composants à bootstraper quand ce module est bootstrapé (typiquement `AppComponent`).

Bien qu'il soit conventionnel de nommer le module racine `AppModule`, il peut porter un autre nom, à condition que `main.ts` (point d'entrée de l'application) bootstrape le bon module.

## Voir aussi

- [[angular-overview]]
- [[composants-angular]]
- [[directives-angular]]
- [[services-angular]]
