---
tags: [azure, app-service, paas]
---

# Azure App Services

Azure App Service est un service basé sur HTTP pour l'hébergement d'applications web, d'API REST et de back-ends mobiles. Plusieurs frameworks et langages de programmation sont pris en charge, et les applications s'exécutent et sont mises à l'échelle facilement dans les environnements Windows et Linux. Dans App Service, une application s'exécute toujours dans un [[azure-app-service-plan|plan App Service]].

## Mise à l'échelle automatique

Intégrée dans Azure App Service, il est possible d'effectuer un **scale-up/down** ou un **scale out/in** :
- **scale-up/down** : Ressources machine (nbre de cœurs, RAM...)
- **scale-out/in** : Nombre d'instances de machine d'exécution

## Intégration et déploiement continus

Le portail Azure offre une [[deploiement-sur-azure-app-service|intégration et un déploiement continus]] immédiats avec :
- **Azure DevOps Services**
- **GitHub**
- **Bitbucket**
- **FTP**
- **Dépôt Git local** sur la machine de développement

## Emplacements de déploiement

Quand vous déployez votre application web, vous pouvez utiliser un emplacement de déploiement différent au lieu de l'emplacement de production par défaut. Les emplacements de déploiement sont des applications en direct avec leurs propres noms d'hôte. Les éléments de contenu et de configuration des applications peuvent être échangés entre deux emplacements de déploiement, y compris l'emplacement de production.

## App Service sur Linux

App Service sur Linux peut aussi héberger des applications web en mode natif sur Linux pour les stacks d'applications prises en charge. En outre, il peut exécuter des conteneurs Linux personnalisés (aussi appelés Web App for Containers). App Service sur Linux prend en charge de nombreuses images intégrées spécifiques à chaque langage.

## Voir aussi

- [[azure-app-service-plan]]
- [[deploiement-sur-azure-app-service]]
- [[azure-web-application]]
