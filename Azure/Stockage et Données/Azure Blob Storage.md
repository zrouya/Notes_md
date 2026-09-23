
Azure Blob Storage est un service Azure permettant de stocker des fichiers binaires (Binary Large OBjects).

Les blobs sont regroupés au sein de **Containers** : 

![[Pasted image 20240808164522.png]]

## Accès aux données

Une fois uploadés, les blobs sont disponibles (si les [[Azure Storage Accounts Authorization|autorisations]] le permettent) à l'URL suivante : 
https://[StorageAccountName].blob.core.windows.net/[ContainerName]/[OptionalVirtualDirectory]/myblob

Note : Les répertoires virtuels proviennent des noms de Container et/ou des Blobs, qui peuvent contenir des '/' pour simuler un arbre de répertoires.

**Note** : Il est possible d'**héberger** un **site web statique** au sein d'un Azure Blob Storage, qui servira les fichiers. Voir [la documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website).

## Gestion tarifaire

La gestion [[Azure Blob Storage Access Tier|niveaux d'accès]] des différents blobs permet d'**optimiser les coûts** de **stockage** et d'**accès** des blobs.

## Protection des données

Dans la section "Data Managment / Data Protection" du [[Azure Storage Accounts|storage account]], il est possible de configurer :
- Le versionning des blobs
- Le soft delete des blobs et des containers
- La journalisation du cycle de vie des blobs (blob change feed)
![[Pasted image 20240811171337.png]]

## Programmation

Il est possible de gérer un Azure Blob Storage au sein d'une [[Azure Blob Storage - DotNet applications|application .Net]].

De même, l'outil CLI **AzCopy** permet de scripter les opérations de gestion du blob Storage.
Voir [la documentation](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10?tabs=dnf) de l'outil.

## Documentation 

Voir [ici](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) la documentation de Azure Blob Storage

**Note** : Pour les différents types de blobs, voir [la documentation](https://learn.microsoft.com/en-us/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json).