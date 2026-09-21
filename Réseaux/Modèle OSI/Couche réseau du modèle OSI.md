
La **couche réseau** du modèle [[Modèle OSI|OSI]] (Network Layer) constitue la **couche 3** du modèle.

Elle fournit des **services de routage** et d'**adressage logique** ([[Adresse IP|Adresses IP]]). Elle est donc considérée comme la **couche de routing**.

Le niveau de granularité des données est le [[Paquets (couche réseau)|paquet]].

La couche réseau manipule 2 types de paquets : 
- Les **paquets de données** :
	La couche réseau **encapsule** les données de la couche 4 ([[Segments (couche transport)|segments]]) avec, entre autres mais principalement, les **adresses IP source** et **destination** du paquet à transmettre.
- Les **paquets de mis à jour du routage (route-update packets)** :
	Ces paquets ne servent pas directement à la transmission de données, mais à la détermination de la meilleure route à emprunter (**path determination**). Ils sont utilisé pour **mettre à jour** les **données de routage** des routeurs voisins (protocoles **RIP**, **OSPF**, **EIGRP**, etc..).

Au niveau de la couche réseau, opèrent les [[Routeurs réseaux|routeurs]], les [[Switch réseau|switches]] multi-layer, le protocole [[Protocole IP (Internet Protocol)|IP]] ([[IPv4]] et [[IPv6]]), ainsi que le protocole [[Internet Control Message Procotol (ICMP)|ICMP]] (ping).