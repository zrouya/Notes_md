---
tags: [docker, images, stockage]
---

# Copy and Write

Le concept de « copy-on-write » (copie sur écriture) est une stratégie de gestion de données utilisée dans les systèmes informatiques, et elle joue un rôle crucial dans la façon dont les [[images-docker|images]] et les [[execution-des-conteneurs-docker|conteneurs]] Docker fonctionnent : gestion plus efficace des modifications de fichiers, optimisation de l'utilisation des ressources, amélioration des performances.

## Fonctionnement de base

Les images Docker sont des modèles **en lecture seule** utilisés pour créer des conteneurs. Lorsque des conteneurs partagent la même image de base, ils **partagent également les couches en lecture seule** de cette image. Cela signifie qu'au lieu de dupliquer les données pour chaque conteneur, **un seul ensemble de données est conservé** sur le disque, économisant ainsi de l'espace.

Lorsqu'un processus dans un conteneur modifie un fichier existant (qui fait partie de l'image de base), le système de gestion de Docker crée **une copie de ce fichier** dans une **nouvelle couche** associée **spécifiquement au conteneur** en cours. Cette nouvelle couche est en **lecture-écriture**, permettant au conteneur de modifier le fichier copié **sans affecter l'image de base ni les autres conteneurs** qui l'utilisent.

Ce mécanisme permet non seulement **d'économiser de l'espace disque**, mais également de **réduire le temps de démarrage** des conteneurs, car il n'est pas nécessaire de copier toutes les données de l'image pour chaque nouveau conteneur. Les **modifications** sont appliquées **uniquement au besoin**, ce qui rend le déploiement des conteneurs plus rapide et plus efficace.

## Voir aussi

- [[images-docker]]
- [[layers-images-docker]]
- [[execution-des-conteneurs-docker]]
