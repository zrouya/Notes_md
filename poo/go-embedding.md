---
tags: [poo, go, composition, heritage]
---

# Go : embedding, pas héritage

**Go n'a pas d'héritage** : pas de classes, pas de `extends`, pas de `super`, pas de méthode virtuelle redéfinissable, pas de `protected`. La FAQ officielle : *« Go n'a pas de hiérarchies de types ; les types se contentent de satisfaire des interfaces. »*

Mais Go a l'**embedding** : de la composition avec du sucre syntaxique.

## Struct embedding = promotion de méthodes

Un champ déclaré **sans nom** voit ses méthodes *promues* dans le method set du type englobant.

```go
type Logger struct{}
func (Logger) Log(msg string) { fmt.Println(msg) }

type Server struct {
    Logger          // embedded (anonyme)
    Addr string
}

s := Server{Addr: ":8080"}
s.Log("démarrage")        // ✅ promu — raccourci pour s.Logger.Log(...)
```

Effets de bord utiles : `Server` **satisfait automatiquement** toute interface que `Logger` satisfaisait. C'est réutilisation de code **+** acquisition de sous-typage — donc l'essentiel des usages de l'héritage.

## La différence décisive : shadowing ≠ overriding

```go
type Base struct{}
func (Base) Name() string    { return "base" }
func (b Base) Greet() string { return "hello " + b.Name() }

type Derived struct{ Base }
func (Derived) Name() string { return "derived" }   // MASQUE, ne redéfinit pas

Derived{}.Greet()   // → "hello base"    (en Java : "hello derived")
```

Aucune [[open-recursion]] : `Greet` promue s'exécute avec un receveur de type `Base`. Pour l'obtenir, il faut injecter le `self` à la main (voir [[open-recursion]]).

## Interface embedding — là, il y a bien hiérarchie

```go
type Reader interface { Read(p []byte) (int, error) }
type Writer interface { Write(p []byte) (int, error) }

type ReadWriter interface {   // ReadWriter est un sous-type de Reader ET de Writer
    Reader
    Writer
}
```

Ce que Go a supprimé, c'est l'héritage **d'implémentation** — pas le sous-typage ni l'héritage de contrats.

## Pièges de l'embedding

- **Ambiguïté** : deux types embarqués exposant `Close()` → l'appel `s.Close()` ne compile plus (il faut qualifier).
- **Fuite d'API** : tous les membres exportés de l'embarqué deviennent visibles sur l'englobant (embarquer `sync.Mutex` non exporté : `mu sync.Mutex`, sinon `Lock()` devient public).
- **Method set et pointeurs** : embarquer `T` vs `*T` change quelles méthodes sont promues (celles à receveur pointeur ne le sont sur `T` que via une variable adressable).
- Ajouter une méthode au type embarqué modifie le method set de l'englobant → forme atténuée de [[fragile-base-class]].

## Voir aussi

- [[heritage-cinq-mecanismes]] — embedding fournit les points 2, 3, 4, pas le 5
- [[composition-delegation]]
- [[heritage-langages-modernes]]
