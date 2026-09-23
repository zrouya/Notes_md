---
tags: [azure, app-service, deploiement]
---

# Déploiement sur Azure App Service

Azure App Service permet de configurer des pipelines de déploiement de manière intégrée.

## Déploiement automatisé

Azure prend en charge le déploiement automatisé directement à partir de plusieurs sources. Les options suivantes sont disponibles :

- **Azure DevOps Services** : vous pouvez pousser (push) votre code vers Azure DevOps Services, générer votre code dans le cloud, exécuter les tests, générer une version à partir du code et, enfin, pousser votre code vers une application web Azure.
- **[[publication-azure-web-app-via-github|GitHub]]** : Connexion directe de dépôts GitHub à Azure. Les changements poussés sur la branche de production sur GitHub sont déployés automatiquement.
- **Bitbucket** : pareil que GitHub.

## Déploiement manuel

Il existe quelques options pour pusher le code manuellement sur Azure :

- **Git** : Les applications **web App Service** proposent une URL Git à ajouter en tant que dépôt distant. Le code pushé est automatiquement déployé.
- **CLI** : `az webapp up` permet d'empaqueter une application et de la déployer. Peut créer une application web App Service si ce n'est pas déjà fait.
- **Déploiement ZIP** : utilisation de `curl` ou un utilitaire HTTP similaire pour envoyer un fichier zip des fichiers d'application à App Service.
- **FTP/S**

Dans tous les cas, il est préférable d'utiliser des [[azure-deployment-slots|environnements de déploiement Azure]].

## Voir aussi

- [[publication-azure-web-app-via-github]]
- [[azure-deployment-slots]]
- [[azure-app-services]]
