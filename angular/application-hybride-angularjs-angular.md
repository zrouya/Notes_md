---
tags: [angular, angularjs, migration]
---

# Application hybride AngularJS / Angular

Dans une application hybride [[angular-overview|Angular]]/[[angularjs|AngularJS]], les deux frameworks coexistent au sein de la même application. AngularJS est la version 1.x d'Angular, tandis qu'« Angular » désigne les versions 2 et ultérieures du framework. La coexistence s'appuie généralement sur le module `@angular/upgrade/static`, qui fournit des services permettant de faire interagir composants et services d'Angular et AngularJS.

## Démarrer une application hybride

1. **Inclure les scripts nécessaires** : les scripts AngularJS et Angular, ainsi que ceux des applications respectives, dans la page HTML.
2. **Configurer le module AngularJS** : comme dans une application AngularJS classique.
3. **Configurer le module Angular** : en incluant `UpgradeModule` dans les imports du module Angular, module qui facilite la mise à niveau d'une application AngularJS vers Angular.
4. **Bootstrapper l'application** : au lieu d'un bootstrap classique, on bootstrape d'abord le module Angular, puis on utilise `UpgradeModule` pour bootstraper le module AngularJS.

## Exemple de module Angular hybride

```js
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { UpgradeModule } from '@angular/upgrade/static';

@NgModule({
  imports: [
    BrowserModule,
    UpgradeModule
  ],
})
export class AppModule {
  constructor(private upgrade: UpgradeModule) { }
  ngDoBootstrap() {
    this.upgrade.bootstrap(document.body, ['myAngularJSApp']);
  }
}
```

Ici, `myAngularJSApp` est le nom du module AngularJS, bootstrappé dans `ngDoBootstrap` du module Angular via `UpgradeModule`. Une fois l'application bootstrappée, on peut utiliser les composants et services d'Angular et AngularJS dans la même application, y compris des composants AngularJS dans des templates Angular et vice versa.

## Voir aussi

- [[angular-overview]]
- [[angularjs]]
