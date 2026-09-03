---
tags: [reseau, wifi, couche2]
---

# CSMA/CA — Collision Avoidance

Méthode d'accès au support du **Wi-Fi** (802.11), en [[Couche de liaison de données du modèle OSI|couche 2]].

## Pourquoi éviter au lieu de détecter

Une radio ne peut pas écouter et émettre **en même temps sur la même fréquence** : son propre signal couvre tout le reste. La détection de collision de [[Carrier Sense Multiple Access - Collision Detection (CSMA-CD)|CSMA/CD]] y est donc impossible.

S'ajoute le **problème du nœud caché** : deux stations peuvent être hors de portée l'une de l'autre tout en étant toutes deux à portée du point d'accès. Chacune croit le support libre, et leurs trames se percutent au niveau du récepteur.

## Le mécanisme

1. Écouter le support ; s'il est occupé, attendre
2. S'il est libre, attendre un délai supplémentaire aléatoire (*backoff*) — c'est lui qui désynchronise les émetteurs
3. Émettre
4. **Attendre un `ACK` explicite** du destinataire ; sans accusé, considérer la trame perdue et retransmettre

L'accusé de réception à chaque trame est la grande différence avec Ethernet, qui n'en émet aucun en couche 2.

## RTS/CTS

Option contre le nœud caché : la station demande la parole (*Request To Send*), le point d'accès l'accorde (*Clear To Send*), et toutes les stations à portée du point d'accès savent alors de se taire — y compris celles qui n'entendaient pas l'émetteur.

Coûteux en surdébit, donc réservé aux trames volumineuses ou aux environnements denses.

## Conséquence pratique

Ces attentes et accusés expliquent qu'un réseau Wi-Fi n'atteigne jamais son débit théorique, et que **le débit s'effondre avec le nombre de stations actives** : le support reste partagé, là où un [[Switch réseau|switch]] donne à chaque port sa propre bande passante.

## Voir aussi

- [[Topologies de réseaux sans fils]]
- [[Couche de liaison de données du modèle OSI]]
- [[Topologie réseau infrastructure]] · [[Topologie réseau ad hoc]]
