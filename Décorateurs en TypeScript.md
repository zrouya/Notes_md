

Les décorateurs sont une fonctionnalité de [[TypeScript]], qui est un sur-ensemble de JavaScript développé par Microsoft.

Un décorateur ajoute des métadonnées ou change le comportement d'un élément (classe,  propriétés, méthodes, accesseurs, paramètres). Les décorateurs utilisent la syntaxe `@` :

```js
function readonly(target: any, key: string, descriptor: PropertyDescriptor) {
   descriptor.writable = false;
   return descriptor;
}

class MaClasse {
   @readonly
   maMethode() {
        console.log('Bonjour !');
        }
    }

const obj = new MaClasse();
obj.maMethode = function() {
   console.log('Salut !');
};
// Erreur: maMethode est en lecture seule
```

Dans cet exemple, le décorateur `@readonly` est utilisé pour rendre la méthode `maMethode` en lecture seule. Toute tentative de modification de `maMethode` entraînera une erreur.

Il s'agit de l'équivalent en [[TypeScript]] des [[Attributs en CSharp|attributs en C#]].
