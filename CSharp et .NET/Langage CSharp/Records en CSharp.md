---
tags: [csharp, dotnet, types]
---

# Records en C#

Type introduit en [[CSharp|C#]] 9 dont le compilateur génère la sémantique de *valeur*. Sert à modéliser des **données immuables**, pas du comportement.

## Syntaxe

```cs
// Forme positionnelle
public record Personne(string Nom, int Age);

// Forme avec corps
public record Personne
{
    public string Nom { get; init; }
    public int Age { get; init; }
}
```

## Membres générés par le compilateur

- Constructeur et propriétés `init`
- `Equals` / `GetHashCode` **structurels**
- Opérateurs `==` et `!=`
- `ToString()` lisible : `Personne { Nom = Zakir, Age = 30 }`
- `Deconstruct` et `<Clone>$`

## Égalité structurelle et expression `with`

```cs
var a = new Personne("Zakir", 30);
var b = new Personne("Zakir", 30);
a == b;                 // true  (avec une class : false)
ReferenceEquals(a, b);  // false

var c = a with { Age = 31 };  // copie non destructive
```

## Héritage

Un `record` ne peut hériter que d'un autre `record`. L'égalité tient compte du type réel via le membre caché `EqualityContract` : un `Employe` n'est jamais égal à une `Personne`.

## Pièges

- **Immuabilité superficielle** : `init` protège la référence, pas le contenu. Une `List<T>` dans un record reste mutable.
- **Égalité des collections référentielle** : deux records contenant des listes au contenu identique ne sont pas égaux.
- `with` fait une **copie superficielle** (`MemberwiseClone`).
- `record` est un *contextual keyword*, pas un mot réservé.
- **EF Core** : déconseillé comme entité (le change tracker suppose des objets mutables à identité par clé). Idéal en revanche pour DTO, commandes/événements CQRS, value objects DDD.

## Voir aussi

- [[Record Struct en CSharp]]
- [[Types Valeur et Types Référence en CSharp]]
- [[CSharp]]
