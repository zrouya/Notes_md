---
tags: [moc, docker]
---

# Volumes et Données

Persistance et gestion des données des conteneurs : volumes, bind mounts, tmpfs et copy-on-write.

## Notes

- [[Volumes Docker]] — emplacement spécial persistant même après suppression du conteneur
- [[Gestion des données Docker]] — commandes `docker system df` / `prune` pour gérer l'espace disque
- [[Bind mounts Docker]] — connexion d'un conteneur à un emplacement de l'hôte non managé par Docker
- [[Bind mounts Linux]] — mappage entre deux localisations du système de fichiers Linux
- [[Tmpfs mounts Docker]] — montage en mémoire volatile pour un conteneur
- [[Copy and Write]] — stratégie copy-on-write utilisée par les images et conteneurs Docker
