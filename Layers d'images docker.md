
Une image Docker est constituée d'un empilement de layers, qui, chacun, représentent une instruction élémentaire (ex: une commande dans le [[Docker files|Dockerfile]]), basé sur les layers précédents.

Chaque layer est **identifée par un SHA**, qui permet au système de déterminer si 2 layers sont identiques.

Ces layers sont **mis en cache** localement, de sorte que 2 layers identiques (même SHA) ne sont pas téléchargés 2 fois sur le même hôte.

Donc si 2 images utilisent le même layer, celui-ci sera **mis en commun** entre les 2 images (ce qui économise l'espace nécessaire, la taille des téléchargements, etc).

Pour retourner la liste des layers d'une image Docker :
``docker image history [image]``
