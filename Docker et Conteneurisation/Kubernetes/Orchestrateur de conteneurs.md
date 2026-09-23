
Un orchestrateur de conteneurs est un **système de gestion du cycle de vie** des conteneurs.

Il permet de créer, lancer, arrêter, supprimer des conteneurs, et gérer les networks, volumes, etc.., dans le but notamment de : 
- **automatiser** le cycle de vie des conteneurs
- pouvoir facilement effectuer des **scale out/in**, ou des **scale up/down**
- gérer les **erreurs de runtime** des conteneurs
- effectuer des **blue/green deployments** (déploiement **sans rupture de service**)
- **monitorer** le fonctionnement des conteneurs
- gérer la **communication** des conteneurs à travers des **réseaux** différents
- s'assurer que les conteneurs s'exécutent sur des **serveurs de confiance**
- gérer les **données sensibles** (secrets, tokens, keys, mots de passe...) et ne les rendre accessibles qu'aux conteneurs autorisés.