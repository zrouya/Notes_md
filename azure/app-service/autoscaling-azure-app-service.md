---
tags: [azure, app-service, scaling]
---

# Autoscaling Azure App Service

Azure fournit des **options de scaling** pour les [[azure-web-application|Web application]] (section "**Settings/scale up-scale out**"), permettant d'ajuster automatiquement les ressources allouées en fonction de la charge.

## Configuration du scaling

- Dans la section **Settings/Scale up (App Service Plan)**, il est possible de choisir un [[niveaux-tarifaires-azure-app-service|niveau tarifaire]] donnant accès à des options de scaling particulières.
- Dans la section **Settings/Scale out (App Service Plan)**, il est possible de choisir un mode de scaling : **Manual scale** (uniquement **le nombre d'instances**), ou **Custom Autoscale**.
- Pour l'option **Custom autoscale**, il est possible d'ajouter et de gérer des **règles de scaling** :
	- Les règles peuvent être **basées sur des metrics** (CPU %, Mémoire, ...)
	- On peut par exemple définir une action (par ex. **ajout d'une nouvelle instance**) qui se déclenche lorsqu'une règle est vérifiée (ex: CPU > 70% pendant 10 minutes)
	- On peut également définir une **durée de cool down** (période pendant laquelle une règle ne peut pas se redéclencher, pour laisser le temps aux nouvelles instances d'absorber de la charge de travail)

![[Pasted image 20240313144022.png]]

## Monitoring

Dans l'onglet "History", on peut trouver des éléments de monitoring du scaling (chart du nombre d'instances, événements de scaling...).

## Voir aussi

- [[azure-app-service-plan]]
- [[niveaux-tarifaires-azure-app-service]]
- [[mise-a-lechelle-azure-app-service]]
