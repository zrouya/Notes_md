---
tags: [dotnet, aspnet-core, middleware]
---

# ApplicationBuilder — .NET Core

L'`ApplicationBuilder` dans [[dotnet-core|.NET Core]] est un élément central du processus de configuration du middleware pour une application [[asp-net-core|ASP.NET Core]].

## Fonctionnement

1. **Rôle dans la configuration de l'application** : l'`ApplicationBuilder` est utilisé dans la méthode `Configure` de la classe `Startup` de l'application. Cette méthode est appelée par le runtime de .NET Core pour configurer le pipeline de traitement des requêtes HTTP.
2. **Pipeline de middleware** : l'`ApplicationBuilder` permet de définir un pipeline de middleware, une séquence de composants de traitement de requête. Chaque middleware peut effectuer des opérations avant et après l'appel du middleware suivant dans le pipeline.
3. **[[methodes-d-extensions-en-csharp|Méthodes d'extension]]** : l'`ApplicationBuilder` utilise des méthodes d'extension pour ajouter des middlewares au pipeline. Ces méthodes d'extension sont souvent fournies par des packages NuGet (`UseMvc`, `UseStaticFiles`, `UseAuthentication`, etc.).
4. **Flux de requête** : lorsqu'une requête HTTP arrive, elle traverse le pipeline de middleware dans l'ordre où ils ont été ajoutés. Chaque middleware a la possibilité de traiter la requête, de la passer au middleware suivant, ou d'arrêter le traitement.
5. **Personnalisation** : les développeurs peuvent créer leurs propres middlewares pour des fonctionnalités personnalisées et les ajouter au pipeline en utilisant `app.Use`.
6. **Configuration conditionnelle** : on peut configurer le pipeline de manière conditionnelle (environnement de développement, test ou production) via des instructions conditionnelles dans la méthode `Configure`.
7. **Ordre des middlewares** : l'ordre dans lequel les middlewares sont ajoutés à l'`ApplicationBuilder` est crucial, car il détermine l'ordre de traitement des requêtes et des réponses.

En résumé, l'`ApplicationBuilder` dans .NET Core est un outil puissant et flexible pour configurer le traitement des requêtes HTTP dans une application ASP.NET Core, permettant une personnalisation détaillée du comportement de l'application en matière de gestion des requêtes.

## Voir aussi

- [[dotnet-core]]
- [[asp-net-core]]
- [[asp-net-core-middlewares]]
- [[methodes-d-extensions-en-csharp]]
