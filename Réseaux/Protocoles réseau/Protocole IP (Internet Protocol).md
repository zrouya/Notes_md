
Le protocole IP opère au niveau de la **[[Couche réseau du modèle OSI|couche réseau]]** (3ème couche) du [[Modèle OSI|modèle OSI]]. Il permet, à l'aide de [[Routeurs réseaux|routeurs]], de connecter plusieurs réseaux de niveau 2 (gérés eux par des switches), à travers des [[Wide Area Network (WAN)|WANs]] (comme internet) : 

![[Pasted image 20240411153145.png]]

1. **Adressage logique et Routage** :
    
    - Chaque dispositif sur un réseau IP a une **adresse unique**, généralement sous forme [[IPv4]] ou [[IPv6]].
    - Le protocole IP utilise ces adresses pour **acheminer** les [[Paquets (couche réseau)|paquets]] de données à travers des réseaux complexes, en utilisant des [[Routage réseau|routeurs et des tables de routage]].

2. **Fragmentation et Réassemblage** :
    
    - IP **divise les messages** plus grands en [[Paquets (couche réseau)|paquets]] plus petits pour faciliter le transport à travers différents réseaux. Le protocole IP **spécifie le format** de ces paquets.
    - À la destination, ces paquets sont réassemblés dans leur format original, mais cet aspect n'est pas géré par le protocole IP, mais plutôt par le [[Protocole TCP|TCP]].

3. **Sans Connexion et Non Fiable** :
    
    - IP est un protocole **sans connexion**, signifiant qu'il n'établit **pas une connexion fixe** entre l'expéditeur et le destinataire avant de transmettre les données.
    - Il ne **garantit pas** non plus la **livraison** des paquets, leur **ordre** ou l'**intégrité** des données. Ces tâches sont déléguées à des protocoles de couches supérieures, comme [[Protocole TCP|TCP]] dans la [[Couche de transport du modèle OSI|couche de transport]].
    - Les paquets sont **envoyés** de manière **indépendante** les un des autres. Ils peuvent **ne pas suivre** la **même route**.

4. **Encapsulation de Données** :
    
    - IP encapsule les segments de données de la couche de transport, en ajoutant son propre en-tête qui contient des informations essentielles pour le routage et la livraison.

5. **Gestion de la Qualité de Service (QoS)** :
    
    - Bien que ce ne soit pas sa fonction principale, IP peut gérer la QoS en utilisant des champs spécifiques dans l'en-tête IP pour déterminer la priorité des paquets.


Le protocole IP agit comme un intermédiaire essentiel entre la couche de liaison de données et la couche de transport dans le modèle OSI.