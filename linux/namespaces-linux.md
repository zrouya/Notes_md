---
tags: [linux, namespaces, kernel]
---

# Namespaces Linux

Les namespaces Linux sont une fonctionnalité du noyau qui permet d'**isoler et de virtualiser** les **ressources système** entre différents **groupes de processus**. Ils permettent de séparer l'isolation des différents types de ressources système, pour une meilleure flexibilité.

## Types de namespaces

- **PID** : isolation des ID de processus. Chaque namespace PID peut avoir son propre processus init (PID 1).
- **Network** : isolation des ressources réseau (interfaces, tables de routage, ports, etc.).
- **Mount** : isolation et virtualisation des points de montage du système de fichiers.
- **UTS** : isolation du nom d'hôte et du domaine.
- **IPC** : isolation des mécanismes de communication inter-processus.
- **User** : isolation des ID d'utilisateur et de groupe. Permet à un processus de s'exécuter avec un UID/GID différent à l'intérieur d'un namespace.
- **Cgroup namespace** : isolation de la vue des control groups, permettant à des processus de voir uniquement les [[control-groups-linux|cgroups]] auxquels ils appartiennent.

Un groupe de processus peut appartenir à **plusieurs namespaces différents** à la fois. Par exemple, un conteneur Docker typique utilise une combinaison de plusieurs namespaces pour isoler l'application du reste du système (mount, PID, Network, IPC, UTS, User).

## Voir aussi

- [[control-groups-linux]]
