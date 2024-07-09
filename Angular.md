
Framework de développement d'applications web, écrit en [[TypeScript]].

Le fonctionnement d'Angular s'articule autour des concepts suivants : 
1. **[[Modules Angular|Modules]]** : Angular est modulaire. Les modules sont des conteneurs pour différentes parties de l'application, comme les composants, les directives, et les services. Le module racine est appelé `AppModule`.
    
2. **[[Composants Angular|Composants]]** : Les composants sont les blocs de base d'une application Angular. Un composant contrôle une partie de l'écran appelée vue
    
3. **[[Directives Angular|Directives]]** : Les directives sont des fonctions qui modifient le DOM (Document Object Model) ou le comportement d'un élément DOM. Angular a trois types de directives : les directives de composants, les directives structurelles et les directives d'attributs.
    
4. **[[Services Angular|Services]]** : Les services sont des classes qui encapsulent la logique métier et les données. Ils peuvent être partagés entre différents composants.
    
5. **[[Routing Angular|Routing]]** : Le routage permet de naviguer entre différentes parties de l'application. Angular utilise un modèle de routage basé sur l'URL.
    
6. **[[Data Binding Angular|Data Binding]]** : Angular utilise le data binding pour synchroniser les données entre le modèle et la vue. Il existe deux types de data binding : one-way binding et two-way binding.
    
7. **[[Injection de dépendances en Angular|Dependency Injection]]** : Angular utilise l'injection de dépendances pour créer et gérer les objets de l'application.
    

[[Structure de fichiers d'une application Angular|Structure]] d'une application Angular:

- `src`: C'est le répertoire où se trouve le code source de l'application.
    - `app`: Ce répertoire contient les modules, composants, services et autres fichiers de l'application.
    - `assets`: Ce répertoire contient les images, les fichiers audio et autres ressources de l'application.
    - `environments`: Ce répertoire contient les fichiers de configuration pour différents environnements (développement, production, etc.).
    - `index.html`: C'est le fichier HTML principal de l'application.
    - `main.ts`: C'est le point d'entrée de l'application. Il initialise le module principal et lance l'application.
    - `styles.css`: Ce fichier contient les styles globaux de l'application.
- `node_modules`: Ce répertoire contient les modules Node.js nécessaires pour l'application.
- `angular.json`: C'est le fichier de configuration d'Angular. Il contient les paramètres de build, de test et de déploiement de l'application.

Lancement d'une application Angular:

- Pour lancer une application Angular, il faut d'abord installer Node.js et npm (Node Package Manager) sur sa machine.
- Ensuite, on peut utiliser la CLI (Command Line Interface) d'Angular pour créer, développer et déployer l'application. La CLI d'Angular est un outil en ligne de commande qui facilite le développement avec Angular.
- Pour créer une nouvelle application Angular, on utilise la commande `ng new`.
- Pour lancer l'application en mode développement, on utilise la commande `ng serve`. Cela démarrera un serveur de développement et ouvrira l'application dans le navigateur par défaut.
- Pour construire l'application pour la production, on utilise la commande `ng build`.

Mécanismes sous-jacents:

- Angular utilise un mécanisme de détection des changements pour synchroniser le modèle et la vue. Ce mécanisme vérifie les modifications apportées aux données et met à jour la vue en conséquence.
- Angular utilise un mécanisme d'injection de dépendances pour créer et gérer les objets de l'application. Cela permet de rendre l'application plus modulaire et plus facile à tester.
