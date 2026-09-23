
Les modules [[Angular|angular]] sont des conteneurs pour différentes parties de l'application, comme les [[Composants Angular|composants]], les [[Directives Angular|directives]], et les [[Services Angular|services]].
Le module racine est appelé `AppModule`. Ce module est obligatoire, même s'il n'est pas obligé se s'appeler `AppModule`.


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

Bien que ce soit une convention de nommer le module racine `AppModule`, vous pouvez le nommer différemment si vous le souhaitez. Cependant, vous devrez vous assurer que le fichier `main.ts` (qui est le point d'entrée de l'application) bootstrap le bon module.