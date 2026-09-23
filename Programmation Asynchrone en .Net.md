## Concepts Clés de l'asynchrone en [[DotNet|.Net]] et [[DotNET Core|.Net Core]]

### 1. **Tasks**

- Représentent des opérations qui peuvent se terminer dans le futur.
- Peuvent être démarrées avec `Task.Run()` ou `Task.Factory.StartNew()`.

### 2. **async et await**

- `async` marque une méthode comme asynchrone.
- `async`peut être utilisé avec des méthodes qui retournent `void`, `Task` ou `Task<T>`.
- La méthode ne bloque pas le thread principal, ou le thread appelant la méthode
- `await` suspend l'exécution jusqu'à ce que la tâche soit terminée, sans bloquer le thread appelant (il est remis à disposition dans le [[Thread Pools en .Net|thread pool]].)

## Gestion des [[Threads en .Net|Threads]]

- Les opérations asynchrones utilisent efficacement les threads.
- Permettent au thread appelant de rester disponible pour d'autres tâches.
## Bonnes Pratiques

- Utiliser `async` et `await` pour un code lisible et efficace.
- Éviter les [[Deadlocks|deadlocks]] avec `.Result` ou `.Wait()`.

## Tâches et Threads

- Les tâches I/O-bound sont souvent exécutées de manière asynchrone sans utiliser un thread supplémentaire.
- Les tâches CPU-bound nécessitent généralement un thread distinct.

## Asynchrone sans `await`

- Exécuter une tâche sans `await` est possible mais nécessite une gestion prudente des exceptions et de l'état.

## Réactivité et Efficacité

- L'asynchronisme améliore la réactivité des applications, en particulier dans les environnements UI et Web.
- Permet une utilisation plus efficace des ressources.
- .NET Core offre généralement de meilleures performances et efficacité, dûes à des optimisations pour la gestion asynchrone.

## Continuations et Task Scheduler

- Les continuations sont utilisées pour exécuter le code après le `await`. Elles encapsulent tout le code après l'appel `await` dans la méthode. Quand la tâche asynchrone est complète, la continuation est alors planifiée pour l'exécution.
- Le Task Scheduler de .NET gère l'exécution des continuations.

## Contexte de Synchronisation

- Important dans les applications avec UI pour s'assurer que les continuations s'exécutent sur le bon thread.
- Moins pertinent dans ASP.NET Core, où il n'y a pas de contexte de synchronisation spécifique, et où il n'y a pas de Thread UI.

### **Réentrance et Sécurité de Thread**

- Ce mécanisme garantit que le code après le `await` peut reprendre en toute sécurité, en préservant l'état et en garantissant que les opérations UI se produisent sur le bon thread, évitant ainsi les problèmes de concurrence et les deadlocks.