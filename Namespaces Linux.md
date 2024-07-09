
Les namespaces Linux sont une fonctionnalité du noyau qui permet d'**isoler et de virtualiser** les **ressources système** entre différents **groupes de processus**.

Les namespaces permettent de **séparer l'isolation** des différent types de ressources système, pour une meilleure flexibilité.

**Types de namespaces** :

- **PID** : Isolation des ID de processus. Chaque namespace PID peut avoir son propre processus init (PID 1).
- **Network** : Isolation des ressources réseau (interfaces, tables de routage, ports, etc.).
- **Mount** : Isolation et virtualisation des points de montage du système de fichiers.
- **UTS** : Isolation du nom d'hôte et du domaine.
- **IPC** : Isolation des mécanismes de communication inter-processus.
- **User** : Isolation des ID d'utilisateur et de groupe. Permet à un processus de s'exécuter avec un UID/GID différent à l'intérieur d'un namespace.
- **Cgroup namespace** : Isolation de la vue des control groups, permettant à des processus de voir uniquement les [[Control groups Linux|cgroups]] auxquels ils appartiennent.

Un groupe de processus peut donc appartenir à **plusieurs namespaces différents** à la fois. Par exemple, un conteneur Docker typique utilise une combinaison de plusieurs namespaces pour isoler l'application du reste du système (mount, PID, Network, IPC, UTS, User).