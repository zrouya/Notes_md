
Le protocole ARP a pour but de **résoudre** la **correspondance** entre une [[Adresse IP|adresse IP]] et une [[Adresses MAC|adresse MAC]] (protocole ARP), ou inversement (protocole RARP pour Reverse ARP).

Il fait partie de la [[Couche réseau du modèle OSI|couche réseau]] (modèles OSI et TCP/IP).

Au sein d'un réseau, lorsqu'un membre veut **connaître l'adresse MAC** d'un autre membre, en connaissant son adresse IP, il envoie sur le réseau une **requête ARP** (qui est un message **broadcast**, envoyé sur **tout le réseau**).
Il reçoit une **réponse ARP** de la part de l'**appareil** qui **possède** l'**adresse IP** demandée.

Cela lui permettra de **retenir l'adresse MAC** correspondant à l'adresse IP, pour une **communication ultérieure**.

En effet, en envoyant des données à une **adresse MAC spécifique**, un [[Switch réseau|switch]] saura **à quel membre spécifique** du réseau envoyer ces donnés.

![[Pasted image 20240410171009.png]]

Un membre d'un réseau dispose donc d'un **cache ARP** de correspondances entre adresses IP et adresses MAC.

Pour consulter ce cache, il suffit d'utiliser la commande ``arp -a`` dans un terminal (fonctionne sur Windows, Linux, et MacOs).

![[Pasted image 20240410171909.png]]

Note : Sur la réponse données par Windows (à gauche), la mention **"type":"dynamic"** correspond au fait que ces correspondances **proviennent d'une réponse** faite à une **requête ARP** sur le réseau (il est en effet possible d'**ajouter manuellement** des correspondances dans ce cache, auquel cas ces entrées **ne seront pas de type "dynamic"**).
