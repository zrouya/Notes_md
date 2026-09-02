---
tags: [poo, conception, composition]
---

# Composition et délégation

La composition seule ne remplace pas l'héritage : c'est **composition + délégation + interface** qui le remplace.

- **Composition** : l'objet *contient* un autre objet (relation has-a).
- **Délégation / forwarding** : l'objet expose des méthodes qui **relaient** les appels à l'objet contenu.
- **Interface** : ce qui restaure la substituabilité perdue.

## Le trio complet (C#)

```csharp
interface IRepo { User Find(int id); }              // 1. interface = polymorphisme

class CachedRepo : IRepo {                          // 2. composition
    private readonly IRepo _inner;
    private readonly Dictionary<int, User> _cache = new();
    public CachedRepo(IRepo inner) => _inner = inner;

    public User Find(int id) =>                     // 3. délégation
        _cache.TryGetValue(id, out var u) ? u : _cache[id] = _inner.Find(id);
}
```

Gain vs `class CachedRepo : SqlRepo` :
- fonctionne avec **tout** `IRepo` (SQL, HTTP, mock) → décorateur empilable ;
- immunisé aux changements internes de `SqlRepo` ([[fragile-base-class]]) ;
- testable sans base de données.

## Le coût : le boilerplate

Déléguer une interface de 30 méthodes = 30 wrappers à écrire et à maintenir. Les langages y répondent différemment ([[heritage-langages-modernes]]) :

```kotlin
// Kotlin : délégation dans le langage
class CachedRepo(private val inner: Repo) : Repo by inner {
    override fun find(id: Int): User = cache[id] ?: inner.find(id)   // seul override utile
}
```

```go
// Go : promotion automatique des méthodes
type CachedRepo struct{ Repo }                    // toutes les méthodes de Repo promues
func (c CachedRepo) Find(id int) User { ... }     // on ne réécrit que celle-ci
```

## Patterns qui sont de la composition déguisée

| Pattern | Remplace |
|---|---|
| **Decorator** | sous-classer pour ajouter un comportement |
| **Strategy** | sous-classer pour varier un algorithme |
| **Bridge** | hiérarchie N×M ([[fragile-base-class]] §4) |
| **Adapter** | sous-classer pour changer une signature |

## Voir aussi

- [[heritage-vs-composition]]
- [[template-method-vs-strategy]]
- [[go-embedding]]
