
ICMP est un protocole de la [[Couche réseau du modèle OSI|couche 3]] (réseau) du [[Modèle OSI|modèle OSI]], faisant partie des protocoles du [[Modèle TCP_IP|modèle TCP/IP]]. Il fonctionne en lien avec le [[Protocole IP (Internet Protocol)|protocole IP]], il a pour but de faire des **rapports d'erreur** à l'[[Adresse IP|IP source]] d'un [[Paquets (couche réseau)|paquet]] en cas d'**erreur de livraison** du paquet (typiquement utilisé par les [[Routeurs réseaux|routeurs]]).

Note : ICMP **rapporte** les **erreurs**, mais n'a **aucun moyen** de les **corriger** (le protocole [[Protocole TCP|TCP]] s'en charge).

Comme le protocole IP, il est **sans connexion** (connectionless).

Ses cas d'utilisation principaux sont : 
- Les **rapports d'erreurs** envoyés par les **routeurs** en cas de problème de livraison de paquets.
- Les **diagnostics** de **connexion** (par les administrateurs réseaux), via des lignes de commandes comme **ping**, **pathping**, et **traceroute**.
- Pour [[IPv6|IPv6]] (avec ICMP version 6), envoi de **Neighbor (ou Routers) solicitation messages** ou des **Neighbor (ou Routers) advertisement messages**. Voir les différents [[Types de messages ICMP|types]] de messages ICMP.

