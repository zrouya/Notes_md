---
tags: [angular, ssr, hydratation]
---

# Hydratation Angular

Après un rendu SSR, Angular **réutilise le DOM** envoyé par le serveur (au lieu de le détruire et de le recréer) et y rattache les écouteurs d'événements et l'état.

## Syntaxe / Exemple

```ts
// app.config.ts
providers: [
  provideClientHydration(
    withEventReplay(),            // rejoue les clics faits avant l'hydratation
    withIncrementalHydration(),   // hydratation par blocs @defer
  ),
]
```

```html
<!-- Hydratation incrémentale : bloc rendu côté serveur, hydraté plus tard -->
@defer (hydrate on viewport) {
  <app-forecast-chart />
}
@defer (hydrate on interaction) { <app-comments /> }
@defer (hydrate never) { <app-static-footer /> }

<!-- Exclure un composant qui manipule le DOM à la main -->
<app-legacy-widget ngSkipHydration />
```

## Détails

- **Sans hydratation** (ancien comportement) : le client effaçait le HTML serveur et re-rendait tout → **flicker** et travail en double.
- **Event replay** : les événements capturés avant l'hydratation (clic sur un bouton encore inerte) sont mis en file puis rejoués.
- **Hydratation incrémentale** : les blocs `@defer (hydrate …)` restent en HTML statique et leur JS n'est chargé qu'au déclencheur (`on viewport`, `on interaction`, `on idle`, `on timer`, `when cond`, `never`) → moins de JS au démarrage.
- **HTTP transfer cache** (activé avec `provideClientHydration`) : les réponses `GET`/`HEAD` de `HttpClient` faites côté serveur sont sérialisées dans la page, et le client ne refait pas l'appel. Paramétrable via `withHttpTransferCacheOptions(...)`.
- **Mismatch** : si le DOM client diffère du DOM serveur (manipulation directe du DOM, HTML invalide comme un `<div>` dans un `<p>`, rendu dépendant de `Date.now()`), Angular lève une erreur `NG0500`-`NG0507`.
- `ngSkipHydration` : le composant (et son sous-arbre) est re-rendu côté client, à utiliser en dernier recours.

## Voir aussi

- [[angular-ssr]]
- [[csr-ssr-ssg]]
- [[angular-ssr-pieges]]
