
Les emplacements de déploiement (deployment slots) Azure permettent d'**attacher à une Web app** un ou plusieurs **environnements de déploiement** facilement.

**Plutôt** que de créer **plusieurs Web app Azure** pour chacun des environnements, et de **déployer chaque version** sur l'environnement correspondant, il est préférable de **créer plusieurs slots** de déploiement **pour une seule Web app**.

La création des slots de déploiement Azure s'effectue dans la section **Deployment/Deployment Slot** du portail Azure.

Une fois créé, le slot de déploiement peut être atteint par Visual Studio lors de la publication de l'application via l'IDE : 

![[Pasted image 20240318092440.png]]

Il est possible de "[[Swap de Deployment Slots Azure|Swapper]]" deux slots de déploiement (notamment pour sécuriser les mises en prod, ce qui permet de rollback facilement l'opération).

Note : Il est possible de définir des **éléments de configuration** comme étant **spécifiques à un deployment slot**. Ces éléments de configuration ne seront pas concernés par le swap entre 2 slots.