---
tags: [typescript, angular, decorateurs]
---

# Décorateurs en TypeScript

Les décorateurs sont une fonctionnalité de [[typescript|TypeScript]] permettant d'ajouter des métadonnées ou de modifier le comportement d'un élément (classe, propriété, méthode, accesseur, paramètre) via la syntaxe `@`. Ils constituent l'équivalent TypeScript des [[attributs-en-csharp|attributs en C#]].

## Exemple

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

Ici, le décorateur `@readonly` rend la méthode `maMethode` en lecture seule : toute tentative de modification entraîne une erreur.

## Usage dans Angular

Angular s'appuie massivement sur les décorateurs pour attacher des [[metadonnees-d-un-composant-angular|métadonnées]] aux classes (`@Component`, `@Directive`, `@Injectable`...) ainsi que pour marquer les [[angular-component-inputs-outputs|propriétés Input/Output]] d'un composant.

## Voir aussi

- [[typescript]]
- [[metadonnees-d-un-composant-angular]]
- [[angular-component-inputs-outputs]]
