
Il est possible de "**Swapper**" deux [[Azure Deployment Slots|slots de déploiement]] Azure, ce qui permet d'intervertir les versions d'applications déployées sur les deux environnements correspondants.

![[Pasted image 20240318092728.png]]

![[Pasted image 20240318092806.png]]

Ceci est particulièrement utile pour **sécuriser les mises en production** applicatives (car il est facile de revenir en arrière en cas de problème).

Voir [les étapes d'un swap](https://learn.microsoft.com/en-us/azure/app-service/deploy-staging-slots?tabs=portal#what-happens-during-a-swap).

Notes :
- Si les 2 versions reposent sur des **bases de données** dont la **structure a changé** entre les 2 versions, il sera nécessaire de produire et **exécuter** un **script de migration** des bases de données **avant d'effectuer le swap** (ce qui peut augmenter la durée d'indisponibilité de l'application).
- Il est possible de **configurer** les étapes de **Warm up** de l'application, assurant le fait que l'**application** soit complètement démarrée et **réactive** après le swap.

