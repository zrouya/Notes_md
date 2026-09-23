---
tags: [dotnet, csharp]
---

# Attributs en C#

En [[csharp|C#]], les attributs sont utilisés pour ajouter des métadonnées à des éléments du code source.

```csharp
[Obsolete("Cette méthode est obsolète. Utiliser NouvelleMethode à la place.")]
public void AncienneMethode()
{
    // ...
}
```

Dans cet exemple, l'attribut `Obsolete` est utilisé pour indiquer que la méthode `AncienneMethode` est obsolète.

## Voir aussi

- [[csharp]]
- [[model-validation-net-core]] — exemple d'attributs de validation
