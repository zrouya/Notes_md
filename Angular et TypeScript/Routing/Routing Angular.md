
Le système de routage en Angular est un mécanisme qui permet de naviguer d'une vue à une autre dans une application tout en gérant l'état de navigation. Voici les concepts clés du routage dans Angular :

1. **[[Module Router Angular|RouterModule]]**:
    
    C'est le module qui fournit les outils nécessaires pour implémenter le routage dans une application Angular.
    
2. **[[Routes Angular|Routes]]**:
    
    Les routes sont des objets qui associent des chemins d'URL à des composants. Une route indique à Angular quel composant doit être affiché lorsque l'utilisateur accède à un certain chemin.
    
3. **[[RouterOutlet Angular|RouterOutlet]]**:
    
    C'est une directive qui sert de conteneur pour les composants à afficher en fonction de la route active.
    
4. **RouterLink**:
    
    C'est une directive qui permet de lier un chemin de navigation à un élément HTML, permettant ainsi la navigation entre différentes routes.
    
5. **RouterLinkActive**:
    
    C'est une directive qui permet de gérer l'ajout de classes CSS aux éléments de navigation en fonction de l'activation de leur route respective.
    
6. **ActivatedRoute**:
    
    C'est un service qui contient les informations sur la route active, y compris les paramètres, l'URL, et les données statiques ou dynamiques associées.
    
7. **Services de navigation**:
    
    Angular fournit le service `Router` pour naviguer entre les vues programmablement via des méthodes telles que `navigate` et `navigateByUrl`.
    
8. **Garde (Guards)**:
    
    Les "guards" sont utilisés pour contrôler l'accès aux routes. Il existe différents types de guards comme `CanActivate`, `CanDeactivate`, `Resolve`, etc., qui permettent de gérer l'autorisation et la validation avant qu'une navigation soit effectuée.
    
9. **Route Lazy Loading**:
    
    Angular permet de charger les modules de route de manière paresseuse (lazy), ce qui signifie que les ressources sont chargées à la demande, améliorant ainsi les performances de démarrage de l'application.
    
10. **Stratégies de location**:
    
    Angular offre plusieurs stratégies pour gérer l'historique de navigation, comme la stratégie `HashLocationStrategy` ou `PathLocationStrategy`.
    
11. **Data et Resolve**:
    
    Des données statiques ou dynamiques peuvent être associées à des routes et être pré-chargées ou résolues avant que la navigation ne soit finalisée.
    
12. **ParamMap**:
    
    C'est un service qui permet de gérer les paramètres d'URL, y compris les paramètres de chemin et les paramètres de requête.

Comment cela fonctionne-t-il ?

Lorsqu'une application Angular démarre, le système de routage utilise la configuration des routes définie par l'application pour présenter la vue initiale. Le système écoute les changements dans l'URL du navigateur pour afficher le composant associé à l'URL déterminée. Les changements peuvent être déclenchés par l'utilisateur en cliquant sur des liens, en retournant à la page précédente, ou dynamiquement via le service `Router`.

Les gardes sont vérifiés avant que la route ne soit activée ou désactivée pour s'assurer que l'utilisateur a la permission de voir la page ou pour résoudre des données nécessaires avant l'affichage du composant.

Le "lazy loading" permet de ne charger les modules de fonctionnalités que lorsqu'ils sont réellement nécessaires, réduisant ainsi le temps de chargement initial de l'application.

Le routage est une partie essentielle d'une application Angular, car il contribue à la gestion de l'affichage des composants et à la navigation de l'utilisateur, tout en aidant à optimiser les performances de l'application.