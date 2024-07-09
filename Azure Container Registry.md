

Azure Container Registry est une [[Resource Azure|ressource]] Azure permettant de mettre à disposition un [[Docker registry|registry]] d'images Docker, à la place de [[DockerHUB]].

Une fois créée, il est possible d'y **configurer** les **clés d'accès (access keys)**, via la section "**Settings/Access Keys**" du portail Azure.
En activant un **Admin user**, on définit un **profil de connexion** au registry Azure, qui pourra **[[Publier une image Docker vers un Azure Container Registry|push]]** et **pull** des [[Images Docker|images Docker]] au sein du registry :

![[Pasted image 20240327114819.png]]

