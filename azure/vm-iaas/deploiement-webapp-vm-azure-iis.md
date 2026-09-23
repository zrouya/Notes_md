---
tags: [azure, vm, iis]
---

# Déploiement d'une webapp sur une VM Azure IIS

Avec [[Internet Information Service (IIS)|IIS]] installé sur un [[Windows Server|serveur Windows]], il est possible de **configurer la publication** d'[[azure-web-application|applications]] depuis un poste de développement vers le serveur IIS hébergeant l'application.

## Étapes de configuration

1. Installer sur le serveur IIS le [[Management Service IIS|service de management]] de IIS, qui permet d'**automatiser** en grande partie les **actions de déploiement** d'applications.

2. Configurer le Management Service pour **autoriser les connexions entrantes** :

![[Pasted image 20240304160407.png]]

Les connexions entrantes utilisent le **port 8172** par défaut. Le service doit être redémarré, après application des modifs (panneau de droite).

3. Vérifier que la **bonne version** du **runtime** nécessaire à l'application est **installée** sur le serveur.

4. Installer [[Web Deploy]] sur le **serveur**.

5. Sur le **poste de développement**, avec **Visual Studio** installé, vérifier que le compte de connexion à Azure est le bon (Tool -> Options -> Azure Service Authentication).

6. Créer un **Publish Profile** Azure en tentant une première publication de l'application :
	- Clique droit sur la solution, puis **"Publish"**
	- Sélectionner **Azure** comme **cible de publication**
	- Sélectionner **Azure Virtual Machine**
	- Sélectionner le **compte Azure**, la [[azure-subscription|subscription Azure]], le [[resource-group-azure|groupe de ressource]], puis la VM.

7. Une fois le Publish Profile créé, **valider la connexion** en éditant le profil de publication :
	- "More actions" -> Edit, puis dans l'onglet "Connections", cliquer sur "Validate connection".
	- Renseigner le **nom d'utilisateur et le mot de passe** de l'**utilisateur admin** (compte administrateur) de la VM (défini lors de la création de la VM dans le portail Azure).
	- Accepter le [[Certificat SSL|certificat]].

Une fois cette configuration implémentée, il suffit d'un clic droit -> "Publish" depuis Visual Studio pour déployer l'application sur la VM Azure IIS.

## Voir aussi

- [[machines-virtuelles-azure]]
- [[azure-web-application]]
