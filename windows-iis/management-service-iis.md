---
tags: [iis, windows, deploiement]
---

# Management Service IIS

Le Management Service est une feature IIS permettant de gérer à distance le serveur, de sécuriser cette gestion et d'automatiser la publication d'applications web sans interruption de service.

## Fonctionnalités

- **Gérer à distance** le [[internet-information-service-iis|serveur IIS]] (configuration, gestion des paramètres de sécurité...).
- Fournir des **fonctionnalités de sécurité** pour la gestion à distance (connexions [[ssl]], authentification par certificats...).
- **Gérer les autorisations** pour les droits d'accès au serveur.
- **Automatiser la publication** et le **déploiement** d'applications web sur un serveur IIS, **sans interruption de service**.

## Activation

- Ce service doit être ajouté via le [[windows-server|Server Manager]] Windows (Manage -> Add roles and features).

![[Pasted image 20240304092106.png]]

- Il écoute le **port 8172** par défaut, pour recevoir les requêtes de déploiement à partir du client (généralement un poste de développement).

## Voir aussi

- [[internet-information-service-iis]]
- [[web-deploy]]
