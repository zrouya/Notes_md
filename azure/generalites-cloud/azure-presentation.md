---
tags: [azure, cloud, generalites]
---

# Azure

Plateforme [[cloud|Cloud]] de Microsoft Azure, offrant une gamme étendue de services et de technologies. Ces services sont utilisables après avoir créé un **compte Azure (Azure account)** — voir la [[gestion-comptes-azure|gestion des comptes]] dans Azure.

**Chacun des services** utilisé par un compte Azure doit donner lieu à la **création** d'une [[resource-azure|Resource Azure]]. Les ressources Azure peuvent être regroupées en [[resource-group-azure|groupes de ressources]] Azure (il s'agit d'un **regroupement logique**, généralement **par application** utilisant les services correspondants).

## Principaux services Azure

1. **Compute** :
	- [[machines-virtuelles-azure|Virtual Machines (VMs)]] : Serveurs virtuels configurables pour une grande variété d'applications.
	- **Azure Kubernetes Service (AKS)** : Gestion de conteneurs pour faciliter le déploiement, la gestion et les opérations des applications basées sur conteneurs.
	- **[[azure-app-services|App Services]]** : Plateforme pour héberger des applications web et des API sans se soucier de l'infrastructure sous-jacente.

2. **Stockage** :
	- **Azure Blob Storage** : Stockage d'objets pour les gros volumes de données non structurées.
	- **Azure File Storage** : Stockage de fichiers basé sur SMB et NFS accessible à partir de n'importe où.
	- **Azure Queue Storage** : Stockage de files d'attente pour la communication entre les composants de l'application.

3. **Bases de données** :
	- **[[azure-sql-database|Azure SQL Database]]** : Base de données relationnelle en tant que service basée sur SQL Server.
	- **Cosmos DB** : Base de données NoSQL offrant une distribution globale et une évolutivité horizontale.

4. **Intelligence Artificielle et Machine Learning** :
	- **Azure Machine Learning** : Plateforme pour la création, le déploiement et la gestion des solutions de machine learning.
	- **Cognitive Services** : Ensemble d'API préconstruites pour l'intégration de l'intelligence artificielle dans les applications.

5. **IoT (Internet des Objets)** :
	- **Azure IoT Hub** : Plateforme permettant de connecter, de surveiller et de gérer des billions de ressources IoT.
	- **Azure IoT Central** : Solution SaaS pour la gestion rapide et sécurisée des dispositifs IoT.

6. **Réseautage** :
	- **Virtual Network** : Fournit un réseau privé dans le cloud.
	- **Azure DNS** : Hébergement de domaine et services de gestion DNS.
	- **Content Delivery Network (CDN)** : Réseau de distribution de contenu pour livrer des données et des applications de façon rapide et fiable.

7. **DevOps et outils pour développeurs** :
	- **Azure DevOps** : Suite d'outils pour le développement de logiciels, y compris le suivi des travaux, le partage de code, et la CI/CD.
	- **GitHub Actions for Azure** : Intégration de GitHub pour automatiser les workflows dans Azure.

8. **Sécurité et identité** :
	- **Azure Entra** : Gestion des identités et des accès.
	- **Azure Security Center** : Plateforme unifiée pour la sécurité et la gestion des risques.

9. **Analytique et Big Data** :
	- **Azure Synapse Analytics** : Service d'analyse big data.
	- **Azure HDInsight** : Plateforme pour le traitement de données massives basée sur des technologies open source.

10. **Intégration et Middleware** :
	- **Azure Logic Apps** : Création d'automatisations de processus d'affaires et workflows.
	- **Event Grid** : Service de gestion des événements.

## Voir aussi

- [[resource-azure]]
- [[resource-group-azure]]
- [[gestion-comptes-azure]]
- [[azure-cli]]
