---
tags: [angular, routing, fondamentaux]
---

# Routing Angular

Le système de routage d'[[angular-overview|Angular]] est un mécanisme qui permet de naviguer d'une vue à une autre dans une application tout en gérant l'état de navigation.

## Concepts clés

1. **[[module-router-angular|RouterModule]]** : module fournissant les outils nécessaires pour implémenter le routage.
2. **[[routes-angular|Routes]]** : objets associant des chemins d'URL à des composants.
3. **[[routeroutlet-angular|RouterOutlet]]** : directive servant de conteneur pour les composants à afficher selon la route active.
4. **RouterLink** : directive liant un chemin de navigation à un élément HTML.
5. **RouterLinkActive** : directive gérant l'ajout de classes CSS aux éléments de navigation selon l'activation de leur route.
6. **ActivatedRoute** : service contenant les informations sur la route active (paramètres, URL, données statiques ou dynamiques).
7. **Service `Router`** : permet de naviguer programmablement (`navigate`, `navigateByUrl`).
8. **Guards** (`CanActivate`, `CanDeactivate`, `Resolve`, etc.) : contrôlent l'accès aux routes et la validation avant navigation.
9. **Lazy loading** : chargement à la demande des modules de route, améliorant les performances de démarrage.
10. **Stratégies de location** : gestion de l'historique de navigation (`HashLocationStrategy`, `PathLocationStrategy`).
11. **Data et Resolve** : données statiques ou dynamiques associées à des routes, pré-chargées ou résolues avant navigation.
12. **ParamMap** : gestion des paramètres d'URL (chemin et requête).

## Fonctionnement

Au démarrage, le système de routage utilise la configuration des routes pour présenter la vue initiale, puis écoute les changements de l'URL du navigateur pour afficher le composant associé (clic sur un lien, navigation arrière, ou appel programmatique du service `Router`). Les guards sont vérifiés avant l'activation ou la désactivation d'une route pour valider les permissions ou résoudre les données nécessaires. Le lazy loading ne charge les modules de fonctionnalités que lorsqu'ils sont réellement nécessaires, réduisant le temps de chargement initial.

## Voir aussi

- [[module-router-angular]]
- [[routes-angular]]
- [[routeroutlet-angular]]
