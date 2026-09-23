
Les machines virtuelles Azure sont un type de [[Resource Azure|ressources Azure]] qui permettent de builder, lancer, et se connecter à des machines virtuelles, à partir d'[[Images Docker|images]] de VM.

En [[Créer une VM Azure|ajoutant une ressource]] de type machine virtuelle à un compte Azure, on dispose d'une VM à laquelle il est possible de se [[Connection à une VM Azure|connecter]].

Il s'agit d'une des **solutions d'hébergement** d'applications Web proposées par Azure : 
Il est possible de créer un web serveur dans une VM, via :
- [[Configurer IIS sur un Web server Windows|Internet Information Service (IIS)]] sur une VM Windows
- 

## Networking

Il est possible de configurer une **[[Adresse IP|IP]] publique** à une VM, ainsi qu'un **[[Domain Name System (DNS)]] name**, pour accéder à cette VM depuis le réseau extérieur (Dans la ressource relative à la VM, dans le sous-menu **"Networking"**).

Il est également possible de **gérer les [[Ports réseaux|ports]] exposés** par la VM en ajoutant des **Inbound port rules** via l'interface Azure Portal.

Par exemple, sur une VM Windows Server avec IIS installé, il convient d'ajouter le port **8172** aux Inbound port rules, dans le but d'accéder au [[Management Service IIS|service de management]] IIS, permettant d'automatiser les déploiements vers cette VM Azure.
