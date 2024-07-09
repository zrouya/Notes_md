
L'outil Docker Compose CLI permet de définir et de gérer des applications multi-conteneurs avec Docker (il génère des commandes Docker CLI qui sont envoyées au Docker daemon).

Il est intégré par défaut dans les installations Docker pour Windows ou Mac.
Pour les installations Linux, cet outil est à installer à part.

Voir https://github.com/docker/compose/releases

Les deux commandes principales sont :
- ``docker compose up`` : configure les volumes/réseaux, et lance les conteneurs
- ``docker compose down`` : arrête les container et les supprime, ainsi que les volumes et réseaux Docker
Le reste des commandes principales sont inspirées des commandes Docker classiques : 
- ``docker compose -d up`` permet d'exécuter les conteneurs en mode détaché (comme pour ``docker container -d run``). Par défaut, la console affichera les logs des différents conteneurs, avec un code couleur aléatoire pour différencier les conteneurs
- ``docker compose logs`` aggrège les logs des différents conteneurs
- ``docker compose ps`` liste les conteneurs en exécution
- ``docker compose top`` liste les processus Docker en exécution sur l'hôte