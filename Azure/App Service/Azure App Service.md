
Azure App Service est un service basé sur HTTP pour l’hébergement :
- d’[[Azure Web Application|applications web]]
- d’API REST
- de back-ends mobiles.

Plusieurs Frameworks et langages de programmation sont pris en charge.
Les applications s’exécutent et sont mises à l’échelle facilement dans les environnements Windows et Linux.
Dans App Service, une application s’exécute toujours dans un [[Azure App Service Plan|plan App Service]].

1. **Prise en charge automatique de la mise à l’échelle**

	Intégrée dans Azure App Service, il est possible d’effectuer un **scale-up/down** ou un **scale out/in**.
	-  **scale-up/down** : Ressources machine (nbre de coeurs, RAM...)
	- **scale-out/in** : Nombre d'instances de machine d'exécution

2. **Prise en charge de l’[[Déploiement sur Azure App Service|intégration et du déploiement continus]]**

	Le portail Azure offre une intégration et un déploiement continus immédiats avec :
	-  **Azure DevOps Services**
	- **GitHub**
	- **Bitbucket**
	- **FTP**
	- **Dépôt Git local** sur la machine de développement.

3. **Emplacements de déploiement**

	Pour déployer une application web, il est préférable d'utiliser des [[Azure Deployment Slots|emplacements de déploiement]].

4. **App Service sur Linux**

	App Service sur Linux peut aussi héberger des applications web en mode natif sur Linux pour les stacks d’applications prises en charge. En outre, il peut exécuter des conteneurs Linux personnalisés (aussi appelés Web App for Containers).