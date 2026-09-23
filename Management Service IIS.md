
Le Management Service est une feature IIS permettant :
- de **gérer à distance** le [[Internet Information Service (IIS)|serveur IIS]] (configuration, gestion des paramètres de sécurité...)
- de fournir des **fonctionnalités de sécurité** pour la gestion à distance (connections [[SSL]], authentification par certificats...)
- de **gérer les autorisations** pour les droits d'accès au serveur
- d'**automatiser la publication**, le **déploiement** d'applications web sur un serveur IIS, **sans interruption de service**.

Ce service doit être ajouté via le [[Windows Server|Server Manager]] Windows (Manage -> Add roles and features): 

![[Pasted image 20240304092106.png]]

Ce service écoute le **port 8172** par défaut, pour recevoir les requêtes de déploiement à partir du client (généralement un poste de développement).

