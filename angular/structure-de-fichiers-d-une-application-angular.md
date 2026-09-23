---
tags: [angular, structure, fondamentaux]
---

# Structure de fichiers d'une application Angular

Une application [[angular-overview|Angular]] générée par la CLI suit une arborescence standardisée, centrée sur le dossier `src`.

## Arborescence

- `src` : répertoire du code source de l'application.
    - `app` : modules, composants, services et autres fichiers de l'application.
    - `assets` : images, fichiers audio et autres ressources statiques.
    - `environments` : fichiers de configuration par environnement (développement, production, etc.).
    - `index.html` : fichier HTML principal de l'application.
    - `main.ts` : point d'entrée de l'application, initialise le module principal et lance l'application.
    - `styles.css` : styles globaux de l'application.
- `node_modules` : modules Node.js nécessaires à l'application.
- `angular.json` : fichier de configuration d'Angular (build, test, déploiement).

## Voir aussi

- [[angular-overview]]
- [[modules-angular]]
