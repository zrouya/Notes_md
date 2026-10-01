---
tags: [angular, routing, navigateur, spa]
---

# Routing Angular et navigateur (API History)

Angular ne remplace pas le navigateur : il s'appuie sur l'**API History** (`pushState`, événement `popstate`) pour gérer les navigations internes sans recharger la page.

## Qui fait quoi ?

| Action | Traitement | Requête serveur ? |
|---|---|---|
| 1er chargement, F5, URL tapée | Le serveur renvoie `index.html`, Angular démarre et **lit l'URL** | ✅ |
| Clic sur `routerLink` | `preventDefault` + `history.pushState()` + changement de composant | ❌ |
| `router.navigate()` | `pushState` + changement de composant | ❌ |
| Précédent / Suivant | Le navigateur émet `popstate`, Angular l'écoute | ❌ |
| `<a href="/x">` classique | Angular n'intervient pas → rechargement complet | ✅ (à éviter) |

```
F5 / URL tapée ─► serveur ─► index.html ─► Angular démarre ─► lit l'URL ─► Router
routerLink / navigate / précédent ────────────────────────────────────────► Router
                                    matching ► guards ► resolvers ► <router-outlet>
```

## Conséquences

- Angular **n'empêche jamais** un rechargement complet : l'app redémarre et reconstruit l'écran à partir de l'URL seule.
- `pushState` modifie URL + historique **sans téléchargement** : favoris et partage fonctionnent.
- Le contenu peut encore déclencher des requêtes (chunks lazy, API), mais jamais la page HTML.

## Déploiement : fallback serveur

Sur F5 de `/produits/42`, le serveur doit renvoyer `index.html` (réécriture « SPA fallback »), sinon 404.

Alternative : `withHashLocation()` → `/#/produits/42`. Fonctionne partout (le fragment n'est pas envoyé au serveur) mais les URL sont moins propres.

## Voir aussi

- [[routing-angular]]
- [[csr-ssr-ssg]]
