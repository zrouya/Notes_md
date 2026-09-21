
Les Azure Storage Accounts sont un type de [[Resource Azure|ressources]] Azure offrant des services de stockage de différents types :
- **[[Azure Blob Storage|Blob storage]]** : pour le stockage d'objets (binaries) comme des **images** ou des **vidéos**
- **Table storage** : pour les données sous forme de tables
- **Azure Queues storage** : pour les messages envoyés ou reçus
- **Azure Files storage** : pour faire du partage de fichiers
- **Azure Data Lake storage Gen2**

Les différents type de storage account sont :
- **Standard General Purpose V2** : type de compte standard pour les blobs, les partages de fichiers, les files d'attente et les tables.
- **Premium block blobs** : compte optimisé pour le storage de blobs
- **Premium Files share** : compte optimisé pour le storage de fichiers
- **Premium page blobs** : compte optimisé pour le storage de pages

Une **option de redondance** doit également être définie, en fonction des besoin en **haute disponibilité** : 
![[Pasted image 20240808153431.png]]


Les [[Azure Storage Accounts Authorization|autorisations]] d'accès aux différents services du Storage Account peuvent être configurées de différentes manières (anonymous acces, Access Keys, Shared Access Signature...)

