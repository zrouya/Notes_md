
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
3. **Couche de session (layer 5)** : Contrôle les sessions de communication entre les applications.
	- Etablissement de connexions, gestion des connexions, fins de connexions.
	- TLS
4. **[[Couche de transport du modèle OSI|Couche de transport (layer 4)]]** : Assure la transmission fiable et efficace des données entre deux points.
	- [[Protocole TCP|TCP]]
	- [[Protocole UDP|UDP]]
5. **[[Couche réseau du modèle OSI|Couche réseau (layer 3)]]** : Gère l'acheminement des paquets de données d'une source à une destination.
	- [[Protocole IP (Internet Protocol)|IP]]
6. **[[Couche de liaison de données du modèle OSI|Couche de liaison de données (Datalink - layer 2)]]** : Responsable de la transmission fiable des données sur un lien physique.
	- Adresses MAC
	- Ethernet
7. **[[Couche physique du modèle OSI|Couche physique (layer 1)]]** : Gère la transmission brute des bits sur le support de transmission.
	- Signaux électriques
	- Signaux lumineux (fibre)
	- Ondes radios
	- ...

## OSI est un modèle, pas une implémentation

OSI n'a **jamais été implémenté tel quel** : c'est un modèle pédagogique. Ce qui tourne réellement sur une machine, c'est la pile [[Modèle TCP_IP|TCP/IP]], en 4 couches. Les deux vocabulaires coexistent — on dit « couche 3 » ou « couche 7 » avec les numéros **OSI**.

| Pile TCP/IP (réelle) | Couches OSI | Qui l'implémente |
|----------------------|-------------|------------------|
| Application | 7 · 6 · 5 | Le code applicatif, la lib HTTP |
| Transport | 4 | Le noyau |
| Internet | 3 | Le noyau, puis les routeurs |
| Accès réseau | 2 · 1 | Le pilote et la carte réseau |

Les couches 5 et 6 sont quasi inexistantes dans la pile réelle : elles sont absorbées par la couche applicative.

## Le principe fondamental

Chaque couche **ne dialogue qu'avec son homologue en face**, et ignore tout de celles du dessous. C'est ce contrat qui permet à HTTP de fonctionner à l'identique sur de la fibre, du Wi-Fi ou de la 5G.

Mécaniquement, cela se traduit par l'[[encapsulation]] : chaque couche ajoute son en-tête sans modifier ce qu'elle a reçu.

## Repères pratiques

- Un **[[Switch réseau|switch]]** travaille en couche 2 (adresses MAC), un **[[Routeurs réseaux|routeur]]** en couche 3 (adresses IP)
- Un **load balancer L4** répartit sur des couples `IP:port` sans ouvrir le contenu ; un **L7** lit l'URL et les en-têtes HTTP
	- Azure Load Balancer = L4 · Application Gateway et Front Door = L7
- Chaque couche a **ses propres pannes** : voir [[erreurs-connexion-econnrefused]] et [[diagnostic-par-couche]]

## Voir aussi

- [[Modèle TCP_IP]] · [[encapsulation]]
- [[_index/reseau|Index — Réseau]]