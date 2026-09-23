---
tags: [poo, csharp]
---

# Méthodes d'extension en C#

Les méthodes d'extension en [[csharp|C#]] sont un mécanisme puissant qui permet d'ajouter de nouvelles méthodes à des types existants sans modifier leur code source ou hériter d'eux. Elles sont particulièrement utiles pour ajouter des fonctionnalités à des classes fermées ou des types intégrés comme `string`, `int`, etc.
C'est une fonctionnalité apparue avec [[csharp|C#]] 3.0, et les versions ultérieures (elle n'est pas dépendante de la version du framework .NET utilisé).

## Fonctionnement

1. **Définition** : une méthode d'extension est définie comme une méthode statique dans une classe statique. Ce qui la distingue est son premier paramètre, qui est précédé du mot-clé `this`. Ce paramètre spécifie le type sur lequel la méthode d'extension peut être appelée.

2. **Syntaxe de base** :
```cs
public static class ExtensionMethods
{
    public static int WordCount(this string str)
    {
        return str.Split(new char[] { ' ', '.', '?' },
				StringSplitOptions.RemoveEmptyEntries).Length;
    }
}
```
Dans cet exemple, `WordCount` est une méthode d'extension sur le type `string`.

3. **Utilisation** : pour utiliser une méthode d'extension, il faut inclure un `using` pour l'espace de noms contenant la classe d'extension. Une fois inclus, on peut appeler la méthode d'extension comme si elle était une méthode membre du type étendu.
```cs
string myString = "Hello, world!";
int count = myString.WordCount(); // Utilisation de la méthode d'extension
```

4. **Avantages** :
    - **Modularité** : les méthodes d'extension permettent d'ajouter des fonctionnalités aux classes sans les modifier.
    - **Réutilisabilité** : elles favorisent la réutilisation du code, car on peut définir une méthode d'extension une fois et l'utiliser sur n'importe quelle instance du type étendu.
    - **Intégration fluide** : elles s'intègrent de manière transparente, semblant être des méthodes du type original.

5. **Limitations** :
    - **Accès aux membres privés** : les méthodes d'extension ne peuvent pas accéder aux membres privés de la classe qu'elles étendent.
    - **Résolution de méthodes** : si une méthode d'instance avec la même signature existe déjà sur le type, cette méthode d'instance aura la priorité sur la méthode d'extension.

6. **Exemples courants** :
    - **LINQ** : les méthodes LINQ sont de bons exemples de méthodes d'extension, car elles ajoutent des fonctionnalités de requête aux types `IEnumerable` et `IQueryable`.
    - **[[asp-net-core-middlewares|ASP.NET Core Middleware]]** : en ASP.NET Core, les méthodes d'extension sont utilisées pour ajouter des middlewares au pipeline de traitement des requêtes.

En résumé, les méthodes d'extension sont un moyen élégant d'ajouter de nouvelles fonctionnalités aux types existants en .NET, améliorant la lisibilité du code et favorisant les principes de programmation propre tels que le DRY (Don't Repeat Yourself) et la séparation des préoccupations.

## Voir aussi

- [[csharp]]
- [[asp-net-core-middlewares]]
