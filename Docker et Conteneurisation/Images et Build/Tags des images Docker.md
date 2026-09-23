
Un tag d'image Docker est un **pointeur** vers un **commit spécifique** de cette image, au sein d'un [[Docker repository]], ou sur la machine locale.

Dans un contexte donné (Docker repository, ou machine locale avec Docker installé), le couple "nom_d_image:tag" **identifie** une image de **manière unique**.

Pour modifier un tag : 
``docker image tag SOURCE_IMAGE[:tag] TARGET_IMAGE[:tag]``
(si les tags, optionnels, ne sont pas renseigné, il s'agit du **tag par défaut : latest**)

Note : puisque l'identification doit être unique, réaffecter à une image un nom et un tag qui appartenaient déjà à une  autre image différente casse le lien avec l'image précédente. Ce tag pointera désormais vers la nouvelle image.