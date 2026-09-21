
Le **Routing Mesh** est une fonctionnalité [[Réseaux Docker Overlay|réseau]] de [[Docker Swarm]], qui permet de router les paquets destinés à un [[Services Docker Swarm|service]] Docker Swarm à travers le réseau overlay, vers un [[Noeuds Swarm|noeud]] exécutant la [[Tasks Docker Swarm|tâche]] correspondante, sans se soucier de quel noeud va effectivement traiter le paquet.

Il s'agit d'un élément crucial de Docker Swarm, pour la **haute disponibilité**, la **balance de charge**, et la **tolérance aux pannes** dans des applications distribuées.

### Routage entre conteneurs (tâches)

Chaque service Docker Swarm est accessible via son **nom DNS**. Pour effectuer le routage d'un paquet destiné à un service Docker Swarm vers une tâche correspondante, Docker Swarm utilise des [[Adresses IP Vituelles (VIP)|IP virtuelles]], qui assurent le **load balancing** vers l'ensemble des tâches exécutant le service. 

![[screenshot-www.udemy.com-2024.04.05-11_24_50.png]]

Cette approche permet d'éviter les soucis de caches DNS à mettre à jour, si on utilisait un [[Round Robin DNS|round robin]] DNS.

### Gestion du flux entrant (Ingress)

![[Pasted image 20240405114000.png]]