
Les [[Azure Blob Storage|blobs]] stockés au sein d'un Azure Blob Storage peuvent appartenir à différents **niveaux tarifaires de stockage (access tier)**, en fonction des besoins en **fréquence d'accès**.

En adaptant les access tier, il est possible :
- De **diminuer les coûts** d'**accès** (mais d'**augmenter les coûts** de **stockage**) des blobs plus **fréquemment** requêtés.
- Et inversement pour les blobs moins fréquemment requêtés.
- D'**archiver** des blobs pour encore réduire les coups de stockage (ces blobs ne seront alors **plus disponible**, avant d'avoir été remis à disposition)

![[Pasted image 20240811170132.png]]

Pour optimiser **dynamiquement** les access tiers des blobs, il faut définir des **Lifecycle Management Policies** au sein d'un Blob Storage, qui permet de **définir des règles** de passage des blobs d'un access tier à l'autre : 

![[Pasted image 20240811170416.png]]

![[Pasted image 20240811170507.png]]