---
tags: [angular, ssr]
---

# SSR Angular (`@angular/ssr`)

Rendu de l'application Angular **côté serveur** (Node.js + Express) : chaque requête produit du HTML complet, ensuite hydraté dans le navigateur.

## Mise en place

```bash
ng new mon-app --ssr            # à la création
ng add @angular/ssr             # sur une appli existante
```

Fichiers générés / modifiés :

```text
src/
├── main.ts                  ← bootstrap navigateur (inchangé)
├── main.server.ts           ← bootstrap serveur
├── server.ts                ← serveur Express (API, fichiers statiques, rendu)
└── app/
    ├── app.config.ts         ← + provideClientHydration(withEventReplay())
    ├── app.config.server.ts  ← config serveur (fusionnée avec app.config)
    └── app.routes.server.ts  ← mode de rendu par route
```

```ts
// app.config.server.ts
const serverConfig: ApplicationConfig = {
  providers: [provideServerRendering(withRoutes(serverRoutes))],
};
export const config = mergeApplicationConfig(appConfig, serverConfig);

// server.ts (simplifié)
const app = express();
const angularApp = new AngularNodeAppEngine();
app.use(express.static(browserDistFolder, { maxAge: '1y', index: false }));
app.use((req, res, next) =>
  angularApp.handle(req)
    .then(r => r ? writeResponseToNodeResponse(r, res) : next())
    .catch(next));
if (isMainModule(import.meta.url)) app.listen(process.env['PORT'] || 4000);
export const reqHandler = createNodeRequestHandler(app);
```

## Build et exécution

```bash
ng build                                       # → dist/mon-app/browser + dist/mon-app/server
node dist/mon-app/server/server.mjs            # serveur de prod (port 4000)
ng serve                                       # le dev server fait aussi le SSR
```

## Détails

- `dist/browser` = assets statiques et pages prérendues. `dist/server` = bundle Node qui rend les pages.
- `reqHandler` permet de déployer sur des plateformes serverless (Firebase, Netlify, Vercel…).
- `server.ts` est un serveur Express classique : on peut y ajouter des routes API, de la compression ou des headers.
- Le serveur attend que l'appli soit **stable** (requêtes HTTP et tâches en attente terminées) avant d'envoyer le HTML.
- Le code s'exécute **dans deux environnements** : voir les pièges dans [[angular-ssr-pieges]].

## Voir aussi

- [[csr-ssr-ssg]]
- [[angular-hydratation]]
- [[angular-ssr-render-modes]]
- [[angular-ssr-pieges]]
