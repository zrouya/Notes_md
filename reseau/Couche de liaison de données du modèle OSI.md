
La couche liaison de données du modèle [[Modèle OSI|OSI]] (Datalink Layer) constitue la **couche 2** du modèle.

Elle est responsable de la transmission des données **au sein d'un même réseau** : elle s'assure que la donnée est transmise **au bon appareil** (device) au sein d'un **LAN**.

Le niveau de granularité des données est la [[Frames (couche liaison de données)|trame (frame)]].

Elle encapsule les données qu'elle reçoit de la couche 3 en y ajoutant les [[Adresses MAC|adresses MAC]].

La couche de liaison de données tient son nom de son rôle de **traduction des données** de la [[Couche réseau du modèle OSI]] ([[Paquets (couche réseau)|paquets]]) à la [[Couche physique du modèle OSI|couche physique]] (**bits**).

Elle est constituée de 2 sous-couches : 
- La couche **Logical Link Control (LLC)** : responsable de la **gestion des erreurs** et du **contrôle de flux** des trames.
- La couche **Media Access Control (MAC)** : responsable de l'**adressage physique** ([[Adresses MAC]]), et de la **topologie logique** ([[Ethernet]], [[Carrier Sense Multiple Access - Collision Detection (CSMA/CD)|CSMA/CD]], [[Carrier Sense Multiple Access - Collision Avoidance (CSMA/CA)|CSMA/CA]]).