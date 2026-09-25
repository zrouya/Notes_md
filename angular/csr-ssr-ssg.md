---
tags: [angular, web, ssr, performance]
---

# Stratégies de rendu web : CSR, SSR, SSG

Où et quand le HTML d'une page est produit : **dans le navigateur** (CSR), **sur le serveur à chaque requête** (SSR) ou **au build** (SSG / prérendu).

## Déroulé

```text
CSR (SPA)  : GET / → index.html vide + bundle JS → JS exécuté → appels API → rendu
SSR        : GET / → serveur Node exécute Angular → appels API → HTML complet
             → affichage immédiat → JS chargé → hydratation (interactif)
SSG        : ng build → HTML de chaque route généré une fois → servi en statique
             → hydratation côté client
```

## Comparatif

| | CSR | SSR | SSG |
|---|---|---|---|
| Premier affichage (FCP/LCP) | Lent (attend le JS) | Rapide | Le plus rapide |
| SEO, aperçus de liens (OpenGraph) | Médiocre | Bon | Bon |
| Données fraîches / par utilisateur | Oui | Oui | Non (figées au build) |
| Infra | Statique (nginx, CDN, `wwwroot`) | **Serveur Node** à héberger | Statique |
| Charge serveur | Nulle | Rendu à chaque requête | Nulle |
| Complexité du code | Faible | Code isomorphe (navigateur et serveur) | Moyenne |

## Quand choisir quoi

- **CSR** : applis derrière une authentification, back-offices, outils métier, dashboards → pas de SEO, données par utilisateur.
- **SSR** : sites publics à contenu dynamique (e-commerce, médias), où le SEO et le premier affichage comptent.
- **SSG** : pages publiques stables (landing, docs, blog).
- **Hybride** : Angular permet de choisir **par route** (cf. [[angular-ssr-render-modes]]).

## Détails

- **Hydratation** : le client réutilise le DOM rendu par le serveur au lieu de le recréer (cf. [[angular-hydratation]]).
- Le SSR améliore l'**affichage**, pas forcément l'**interactivité** : entre l'affichage et l'hydratation, la page est visible mais inerte (gap TTI). Angular le compense par l'*event replay*.

## Voir aussi

- [[angular-ssr]]
- [[angular-hydratation]]
- [[angular-ssr-render-modes]]
