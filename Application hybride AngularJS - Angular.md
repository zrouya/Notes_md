
Dans une application hybride [[Angular]]/[[AngularJS]], les deux frameworks coexistent dans la même application. [[AngularJS]] est la version 1.x d'[[Angular]], tandis qu'Angular fait référence aux versions 2 et ultérieures du framework.

Pour faire coexister Angular et AngularJS dans la même application, vous utilisez généralement le module `@angular/upgrade/static` d'Angular. Ce module fournit des services qui permettent de faire interagir les composants et services d'Angular et AngularJS.

Voici comment vous pouvez démarrer une application hybride Angular/AngularJS :

1. **Inclure les scripts nécessaires** : Vous devez inclure les scripts AngularJS et Angular dans votre page HTML. Vous devrez également inclure les scripts de votre application AngularJS et Angular.
    
2. **Configurer le module AngularJS** : Configurez votre module AngularJS comme vous le feriez normalement dans une application AngularJS.
    
3. **Configurer le module Angular** : Configurez votre module Angular en incluant `UpgradeModule` dans les imports de votre module Angular. `UpgradeModule` est un module d'Angular qui facilite la mise à niveau d'une application AngularJS vers Angular.
    
4. **Bootstrapper l'application** : Au lieu de bootstrapper votre application AngularJS ou Angular de manière normale, vous bootstrapperez les deux en utilisant `UpgradeModule`. Vous devez d'abord bootstrapper le module Angular, puis utiliser `UpgradeModule` pour bootstrapper le module AngularJS.
    

Voici un exemple de configuration d'un module Angular dans une application hybride :

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

Dans cet exemple, `myAngularJSApp` est le nom du module AngularJS. Vous pouvez voir que le module AngularJS est bootstrappé dans la méthode `ngDoBootstrap` du module Angular en utilisant `UpgradeModule`.

Une fois que l'application est bootstrappée, vous pouvez utiliser les composants et services d'Angular et AngularJS dans la même application. Vous pouvez même utiliser des composants AngularJS dans des templates Angular et vice versa.