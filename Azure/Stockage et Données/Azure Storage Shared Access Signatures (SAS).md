
Les Shared Access Signatures (SAS) sont des **URL** permettant d'accéder à des ressources d'un [[Azure Storage Accounts|Azure Storage account]], en y associant des **droits d'accès** (lecture, écriture, ...).

On peut définir une shared access signature au niveau d'un **[[Azure Blob Storage|blob]]** particulier, ou d'un **container** :

![[Pasted image 20240811160423.png]]

![[Pasted image 20240811162230.png]]
Note : Pour les Shared Access Signatures au niveau **Blob** ou **Container Blob storage**,  il est possible de rattacher à une SAS une **Stored Access Policy**, qui permet de **gérer dynamiquement** les restrictions d'accès associées à la SAS.


On peut aussi générer une SAS au niveau d'un Storage Account entier (là on définit quels services et quels droits sont accessibles) :

![[Pasted image 20240811161048.png]]

