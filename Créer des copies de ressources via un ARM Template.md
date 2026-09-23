
Au sein d'un fichier de [[ARM Templates|template]] ARM, il est possible d'utiliser la propriété ``copy:`` pour créer des copies d'une [[Resource Azure|ressource]].

Exemple de fichier de template : 
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "name": "[concat(copyIndex(),'appstore46565634')]",
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2021-04-01",            
            "location": "North Europe",
            "kind": "StorageV2",
            "sku": {
                "name": "Standard_LRS"
            },
            "copy" : { 
                "name" :"storagecopy",
                "count" : 3
            }
        }
    ]    
}
```

Ici, la propriété ``copy:`` permet de spécifier la création de 3 copies de la ressource Azure Storage Account.

Note : Pour les ressources nécessitant un **nom unique** (comme c'est le cas ici), il est nécessaire de spécifier une **expression** dynamique pour le nom de la ressource (ceci se fait via les crochets [ ]).

Dans l'expression, il est possible d'utiliser des **fonctions** de manipulation de **chaînes de caractères**, comme ``concat()`` pour former un nom de ressource.

La propriété **copy** donne accès à la fonction ``copyIndex()`` qui retourne l'**indice de base zéro** de la copie créée. On obtient donc un **nom dynamique unique** pour chaque copie de la ressource.