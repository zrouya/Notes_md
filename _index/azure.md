# Azure — Map of Content

## API Management (APIM)

- [[apim-rate-limiting]] — 4 niveaux de granularité, familles rate-limit vs quota, cumul et caveat multi-instance
- [[apim-caching]] — Vue d'ensemble du cache APIM : mode interne vs externe (Redis)
- [[apim-cache-policies]] — Politiques cache-lookup, cache-store, cache-remove-value et leurs paramètres
- [[apim-cache-bonnes-pratiques]] — Bonnes pratiques, limites, et approche ETag manuelle

## Monitoring & Observability

> Concepts transverses (OpenTelemetry, Grafana, Tempo, piliers…) : voir [[_index/observabilite|l'index Observabilité]].

- [[azure-monitor-cartographie]] — **Vue d'ensemble** : ombrelle Azure Monitor, stores vs sources vs produits, confusions à dissiper
- [[appinsights-principe]] — Principe général d'Application Insights : connection string, flux de données, SDK vs codeless
- [[appinsights-auto-instrumentation]] — Auto-instrumentation codeless sur Azure Container Apps (.NET et Java sans modifier le code)
- [[container-apps-otel-agent]] — Agent OpenTelemetry managé au niveau de l'Environment : routage par signal, contraintes
- [[container-apps-app-logs]] — Modèle de logs Container Apps (`appLogsConfiguration`) : pourquoi pas de diagnostic settings, tables `_CL`
- [[log-analytics]] — Le magasin de logs Azure Monitor (tables, KQL) ; App Insights écrit dedans
- [[log-analytics-cout]] — Modèle de coût (ingestion au Go), leviers de réduction
- [[diagnostic-settings]] — Router les logs/métriques d'une ressource vers Log Analytics
- [[apim-gateway-logs]] — Table `ApiManagementGatewayLogs` : une ligne par requête, split `TotalTime`/`BackendTime`
- [[appinsights-tables-requests-dependencies]] — Tables `AppRequests` / `AppDependencies` et reconstruction d'une trace

## Identity & IAM

- [[managed-identity-system-assigned]] — Identité liée au cycle de vie d'une ressource, non partageable
- [[managed-identity-user-assigned]] — Identité indépendante, attachable à plusieurs ressources
- [[managed-identity-comparaison]] — Tableau comparatif System vs User Assigned
- [[entra-graph-permissions]] — Permissions Microsoft Graph : délégué (`scp`) vs application (`roles`), consentement
- [[entra-directory-roles]] — RBAC sur l'annuaire : actions `microsoft.directory/*`, scoping par AU, PIM
- [[entra-permissions-vs-roles]] — Différences conceptuelles des deux référentiels, recouvrement et règle de choix
- [[entra-app-registration-vs-enterprise-app]] — Objets `application` vs `servicePrincipal`, relation 1→N, cas des managed identities
- [[entra-audit-permissions-sp]] — Vérifier les permissions réelles d'un compte de service (portail + CLI), pièges `Group.Create`
- [[azure-entra]] — Vue d'ensemble d'Azure Entra (anciennement Azure Active Directory) comme fournisseur d'identités

## App Service

- [[azure-app-services]] — Vue d'ensemble d'Azure App Service : hébergement web/API, scaling, CI/CD, Linux
- [[azure-app-service-plan]] — Le plan App Service comme unité d'échelle des applications
- [[niveaux-tarifaires-azure-app-service]] — Catégories de niveaux tarifaires (partagé, dédié, isolé)
- [[autoscaling-azure-app-service]] — Configuration des règles de scale-up/scale-out
- [[mise-a-lechelle-azure-app-service]] — Comportement de mise à l'échelle selon le niveau tarifaire
- [[azure-deployment-slots]] — Emplacements de déploiement (deployment slots)
- [[swap-deployment-slots-azure]] — Swapper deux deployment slots
- [[azure-web-application]] — La ressource Web App et ses fonctionnalités de management
- [[deploiement-sur-azure-app-service]] — Déploiement automatisé et manuel
- [[publication-azure-web-app-via-github]] — Déploiement via un repository GitHub
- [[azure-app-service-logging]] — Configuration et consultation des logs
- [[azure-app-configuration]] — Paires clé/valeur centralisées pour une application
- [[azure-feature-flags]] — Feature flags Azure App Configuration
- [[domain-names-azure-web-app]] — Gestion des noms de domaine personnalisés
- [[certificat-ssl-azure-web-app]] — Certificat SSL pour le HTTPS

## Functions

- [[azure-function-app]] — Ressource PaaS hébergeant des fonctions serverless
- [[azure-functions]] — Fonctions individuelles et types de trigger
- [[http-trigger-azure-function]] — Fonction déclenchée par HTTP

## Conteneurs

- [[azure-container-group]] — Regroupement de conteneurs partageant le même hôte
- [[azure-container-instances]] — Déploiement de conteneurs Docker sans gestion d'infrastructure
- [[azure-container-registry]] — Registry d'images Docker Azure
- [[publier-image-docker-azure-container-registry]] — Push d'une image vers l'ACR
- [[azure-kubernetes-service]] — Déploiement de clusters Kubernetes managés

## VM / IaaS

- [[creer-vm-azure]] — Création d'une VM Azure
- [[connexion-vm-azure]] — Connexion RDP/SSH/Bastion à une VM
- [[machines-virtuelles-azure]] — Vue d'ensemble des VM Azure, hébergement web, networking
- [[deploiement-webapp-vm-azure-iis]] — Publication d'une application vers une VM IIS

## Stockage & Bases de données

- [[azure-storage-accounts]] — Types de storage account et de stockage
- [[azure-blob-storage]] — Stockage d'objets binaires
- [[azure-sql-database]] — Base de données relationnelle managée
- [[dtu-sql-azure]] — Modèle de facturation DTU
- [[vcore-azure]] — Modèle de facturation vCore
- [[sql-elastic-pool-azure]] — Mutualisation de ressources SQL entre bases

## Comptes & IAM

- [[gestion-comptes-azure]] — Notion d'Azure Account et ses rattachements
- [[azure-subscription]] — Subscription Azure
- [[resource-azure]] — Notion de ressource Azure
- [[resource-group-azure]] — Regroupement logique de ressources

## Généralités Cloud

- [[azure-presentation]] — Présentation générale de la plateforme Azure et de ses services
- [[cloud]] — Concept de Cloud
- [[platform-as-a-service-paas]] — Modèle PaaS
- [[immutable-infrastructure]] — Principe d'infrastructure immuable
- [[azure-cli]] — Outil CLI multiplateforme d'administration Azure

## Gestion & Administration (PowerShell)

- [[azure-powershell]] — Module PowerShell de gestion des ressources Azure
- [[installation-azure-powershell]] — Installation du module
- [[commandes-powershell-azure]] — Cmdlets classées par module et type de ressource
- [[azure-powershell-deployment-slots]] — Gestion des deployment slots via script
- [[azure-powershell-deploiements-github]] — Déploiement GitHub via script PowerShell
