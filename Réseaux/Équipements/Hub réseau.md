
Les Hubs ne sont plus utilisés dans les réseaux modernes, ils ont été remplacés par les [[Switch réseau|switches]].

Généralement utilisés comme noeud central d'un [[Topologie réseau en étoile|réseau en étoile]], son rôle était de connecter plusieurs membre d'un réseau, en **broadcastant** les signaux :

Le signal arrivant sur un port du HUB était **répété** à **l'ensemble des autres ports** indistinctement (d'où le statut de **répéteur multi-ports**).

Ce fonctionnement (considéré comme de [[Couche physique du modèle OSI|couche 1]] du modèle OSI, d'où l'appellation de **dumb device**), peut facilement causer des **erreurs de collision** réseau (si 2 membres du réseau veulent envoyer des données en même temps, ils se retrouvent avec des trames entrantes et sortantes en même temps) : les données transférées peuvent **être corrompue** (problème d'**intégrité de données**).

De même, ce type d'appareil pose des questions de **sécurité**, dans la mesure où les **données envoyées** par un membre se **propagent à tous les membres** connectés au HUG

![[Pasted image 20240222105711.png]]