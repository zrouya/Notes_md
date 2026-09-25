---
tags: [angular, ssr]
---

# Pièges du SSR Angular (code isomorphe)

Avec le SSR, les composants et services s'exécutent **aussi dans Node.js**, où `window`, `document`, `localStorage` ou `navigator` n'existent pas.

## Syntaxe / Exemple

```ts
import { afterNextRender, inject, PLATFORM_ID, REQUEST } from '@angular/core';
import { isPlatformBrowser, DOCUMENT } from '@angular/common';

export class ChartComponent {
  private platformId = inject(PLATFORM_ID);

  constructor() {
    // 1. Code DOM / navigateur : exécuté UNIQUEMENT côté client, après rendu
    afterNextRender(() => {
      const saved = localStorage.getItem('zoom');
      this.initChartLib();                 // lib qui touche window/canvas
    });
  }

  load() {
    // 2. Branchement explicite
    if (isPlatformBrowser(this.platformId)) { /* ... */ }
  }
}

// 3. Accéder à la requête entrante côté serveur (cookies, headers)
const req = inject(REQUEST, { optional: true });  // null côté navigateur
const cookie = req?.headers.get('cookie');

// 4. Abstraction DOM compatible serveur
const doc = inject(DOCUMENT);                      // plutôt que `document`
```

## Pièges courants

- **API navigateur** : `window`, `localStorage`, `setInterval` à longue durée… Il faut les déplacer dans `afterNextRender` / `afterEveryRender` (jamais exécutés côté serveur).
- **URLs relatives** : côté serveur, `HttpClient.get('/api/x')` doit être résolu. Angular utilise l'URL de la requête entrante, mais un proxy de dev n'existe pas en prod SSR : il faut prévoir un reverse proxy ou une URL absolue d'API.
- **Authentification** : le navigateur envoie ses cookies à Node, mais Node ne les retransmet **pas** automatiquement à l'API. Il faut un intercepteur qui lit `REQUEST` et propage le header. Un token en `localStorage` est invisible côté serveur, donc la page rendue est « non connectée ».
- **Stabilité** : le serveur attend la fin des tâches en attente. Un `interval` / `timer` infini ou une websocket ouverte au démarrage bloquent ou ralentissent le rendu. Pour les tâches async hors `HttpClient`, utiliser `PendingTasks`.
- **État singleton** : un service `providedIn: 'root'` est recréé **par requête** côté serveur, mais une variable de module (hors DI) est **partagée entre utilisateurs**, ce qui peut causer une fuite de données.
- **Non-déterminisme** : `Math.random()`, `Date.now()` ou le fuseau horaire du serveur différent du client provoquent un mismatch d'hydratation (cf. [[angular-hydratation]]).
- **Libs tierces** qui touchent le DOM à l'import : il faut les importer dynamiquement (`await import()`) dans `afterNextRender`.

## Voir aussi

- [[angular-ssr]]
- [[angular-hydratation]]
- [[injection-de-dependances-en-angular]]
