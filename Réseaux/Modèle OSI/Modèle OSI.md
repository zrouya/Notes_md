
Le modèle OSI (**Open systems interconnection**) est une **norme de communication** de tous les **systèmes informatiques** en **réseau**.
Le modèle OSI est composé de sept couches, chacune ayant des fonctions spécifiques :

1. **Couche applicative (layer 7)** : Fournit des services de réseau aux applications de l'utilisateur final.
	- HTTP
	- FTP
	- gRPC
	- ...
2. **Couche de présentation (layer 6)** : Traduit les données pour assurer l'interprétation correcte entre les systèmes.
	- [[Encodage|Encodage]]
	- Sérialisation/Désérialisation
	- Compression/Décompression
	- ...
1. **Couche de session (layer 5)** : Contrôle les sessions de communication entre les applications.
	- Etablissement de connexions, gestion des connexions, fins de connexions.
	- TLS
2. **[[Couche de transport du modèle OSI|Couche de transport (layer 4)]]** : Assure la transmission fiable et efficace des données entre deux points.
	- [[Protocole TCP|TCP]]
	- [[Protocole UDP|UDP]]
1. **[[Couche réseau du modèle OSI|Couche réseau (layer 3)]]** : Gère l'acheminement des paquets de données d'une source à une destination.
	- [[Protocole IP (Internet Protocol)|IP]]
2. **[[Couche de liaison de données du modèle OSI|Couche de liaison de données (Datalink - layer 2)]]** : Responsable de la transmission fiable des données sur un lien physique.
	- Adresses MAC
	- Ethernet
3. **[[Couche physique du modèle OSI|Couche physique (layer 1)]]** : Gère la transmission brute des bits sur le support de transmission.
	- Signaux électriques
	- Signaux lumineux (fibre)
	- Ondes radios
	- ...