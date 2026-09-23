---
tags: [azure, app-service, logging]
---

# Azure App Service Logging

Il est possible de configurer les options de logging d'une [[azure-web-application|Web app]] Azure via la section "**Monitoring/App Service logs**" du portail Azure.

![[Pasted image 20240312174021.png]]

## Types de logs

- Les **Application logs** : Logs générés au sein du code de l'application
- Les **Web Server logs** : Logs des [[Requêtes HTTP|requêtes HTTP]]
- Les **Detailed error messages** : Copie des pages d'erreur renvoyées au navigateur client
- Les **Deployment logs** : Logs de [[deploiement-sur-azure-app-service|publication]] de l'application

## Consultation en temps réel

Il est possible de consulter ces logs en temps réel dans la section **Monitoring/Log Stream** :

![[Pasted image 20240312174930.png]]

## Voir aussi

- [[azure-web-application]]
- [[deploiement-sur-azure-app-service]]
