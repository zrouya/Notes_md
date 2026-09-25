---
tags: [angular, ssr, ssg, routing]
---

# Modes de rendu par route (SSR hybride)

Avec `@angular/ssr`, chaque route choisit son mode de rendu : **Server** (SSR), **Prerender** (SSG au build) ou **Client** (CSR).

## Syntaxe / Exemple

```ts
// app.routes.server.ts
import { inject } from '@angular/core';
import { PrerenderFallback, RenderMode, ServerRoute } from '@angular/ssr';

export const serverRoutes: ServerRoute[] = [
  { path: '', renderMode: RenderMode.Prerender },            // landing statique
  { path: 'login', renderMode: RenderMode.Client },          // pur CSR
  { path: 'dashboard/**', renderMode: RenderMode.Client },   // espace authentifié
  {
    path: 'articles/:slug',
    renderMode: RenderMode.Prerender,
    async getPrerenderParams() {                             // routes dynamiques à prérendre
      const slugs = await inject(ArticleService).allSlugs();
      return slugs.map(slug => ({ slug }));
    },
    fallback: PrerenderFallback.Server,                      // slug inconnu → SSR
  },
  { path: 'search', renderMode: RenderMode.Server,
    headers: { 'Cache-Control': 'no-store' } },              // headers de réponse
  { path: 'old-page', renderMode: RenderMode.Server, status: 301 },
  { path: '**', renderMode: RenderMode.Server },             // défaut
];
```

## Détails

- `RenderMode.Server` : rendu à chaque requête (données fraîches, personnalisation).
- `RenderMode.Prerender` : HTML généré au `ng build` dans `dist/browser`, servi comme un fichier statique.
- `RenderMode.Client` : le serveur renvoie le shell vide, et tout est rendu dans le navigateur.
- `getPrerenderParams` s'exécute **au build** en contexte d'injection (on peut y utiliser `inject()`).
- `PrerenderFallback` : `Server` (défaut), `Client` ou `None` (404) pour les paramètres non prérendus.
- Ces routes serveur **complètent** les routes Angular (`app.routes.ts`) sans les remplacer : on garde un seul router.
- Une appli sans SSR peut quand même faire du prérendu pur (`outputMode: "static"` dans `angular.json`).

## Voir aussi

- [[angular-ssr]]
- [[csr-ssr-ssg]]
- [[routes-angular]]
