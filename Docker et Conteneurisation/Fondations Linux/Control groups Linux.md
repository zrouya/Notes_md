
Les cgroups (Control Groups) sont une fonctionnalité du kernel Linux qui permet de **limiter, comptabiliser, et isoler l'utilisation des ressources** (CPU, mémoire, E/S disque, etc.) des groupes de processus. Ils sont utilisés pour organiser les processus en groupes hiérarchiques et attribuer des politiques de ressources à ces groupes.

**Fonctionnement** :

- **Hiérarchie** : Les cgroups sont organisés en arborescences hiérarchiques, où chaque arborescence représente un sous-ensemble de ressources à contrôler.
- **Contrôleurs** : Chaque arborescence est associée à un ou plusieurs contrôleurs (cpu, memory, blkio, etc.) qui gèrent l'allocation des ressources.
- **Gestion** : Les cgroups sont gérés via un système de fichiers virtuel (habituellement monté dans `/sys/fs/cgroup`). Les administrateurs peuvent créer des répertoires (cgroups) dans ce système de fichiers, chaque répertoire contrôlant un groupe de processus.

**Gestion des cgroups par l'utilisateur root** :

- **Création et configuration** : Créer des répertoires dans `/sys/fs/cgroup` pour définir de nouveaux cgroups et configurer les limites de ressources en écrivant dans les fichiers de configuration correspondants.
- **Ajout de processus** : Ajouter des processus aux cgroups en écrivant leurs PID dans le fichier `tasks` du cgroup correspondant.
- **Surveillance** : Utiliser les fichiers dans le système de fichiers cgroup pour surveiller l'utilisation des ressources et les statistiques.