
Les modules [[Angular|angular]] sont des conteneurs pour différentes parties de l'application, comme les [[Composants Angular|composants]], les [[Directives Angular|directives]], et les [[Services Angular|services]].
Le module racine est appelé `AppModule` par convention (ce n'est pas une nécessité). Ce module était  obligatoire avant les **Standalone** components.


Voici à quoi ressemble généralement un `AppModule` :
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

- `declarations`: Liste des composants, directives et pipes qui appartiennent à ce module.
- `imports`: Liste des modules dont les composants exportés sont nécessaires dans les templates de ce module.
- `providers`: Liste des providers de dépendances qui seront instanciés par l'injecteur de ce module.
- `bootstrap`: Liste des composants qui doivent être bootstrapés quand ce module est bootstrapé.

Le composant spécifié dans le tableau `bootstrap` est le composant racine de l'application. Dans la plupart des applications, cela sera le composant `AppComponent`.

Bien que ce soit une convention de nommer le module racine `AppModule`, il est possible de spécifier un autre module racine qui servira à bootstaper l'application (dans le fichier ``main.ts``).

Pour les modules autres que le module racine, la configuration est sensiblement la même, sauf que l'option bootstrap n'est pas nécessaire :
```typescript
import { NgModule } from '@angular/core';
import { SomeComponent } from './app.component';

@NgModule({
	declarations: [
		SomeComponent 
	], 
	imports: [
		BrowserModule
	],
	exports: [],
	providers: []
})

export class SomeModule { }
```

``exports``: Liste des composants, directives, pipes, .... qui sont rendus accessibles "de l'extérieur", c'est à dire au sein du module ou composant qui utilise ce module.