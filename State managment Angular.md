
Avant la version 16 d'Angular, la gestion de l'état des composant était assuré par Zone.js : 

Le package Zone.js détermine des "zones" autour des composants de l'application angular, qui représentent le périmètre à mettre à jour (donc les éléments du DOM associés) lors d'une modification de données.

A partir de la version 16 d'Angular (stabilisé en version 17), ce système peut être remplacé par [[Angular Signals|Signals]], système basé sur une gestion évenementielle (souscription aux événements de modification des données).


