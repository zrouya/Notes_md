---
tags: [iis, windows]
---

# Configurer IIS sur un serveur Windows

Pour installer un Web Server sur un [[windows-server|serveur Windows]] — en l'occurrence [[internet-information-service-iis|Internet Information Service, IIS]] —, on ajoute le rôle correspondant via le Server Manager.

## Installation via le Server Manager

- Via le **Server Management Dashboard**, il est possible d'ajouter des **rôles** et des **fonctionnalités** au [[windows-server|serveur]].

![[Pasted image 20240229155159.png]]

- Parmi les rôles proposés à l'ajout, sélectionner le rôle "**Web Server (IIS)**" (via "Manage" -> "Add roles and features" en haut à droite).

![[Pasted image 20240229155543.png]]

## Déploiement

- Une fois le rôle ajouté, il est possible de [[Déploiement d'une webapp sur une VM Azure IIS|configurer le déploiement]] automatique d'applications Web sur ce serveur IIS.

## Voir aussi

- [[internet-information-service-iis]]
- [[windows-server]]
