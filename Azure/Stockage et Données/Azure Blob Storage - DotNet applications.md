

Pour utiliser les service [[Azure Blob Storage]] au sein d'une application [[DotNet]], il convient d'installer le package ``Azure.Storage.Blobs`` : 
```dotnet
dotnet add package Azure.Storage.Blobs
```

Il est possible d'interagir avec le blob storage, les containers, et les blobs via le code applicatif :

```csharp
using Azure.Storage.Blobs;

// ConnectionString à récupérer depuis les Access Keys du Storage Account 
string connectionString =
"DefaultEndpointsProtocol=https;AccountName=appstore4434434;AccountKey=PN5HdpUFsovnw3l05S0s4OqnpfFzxYf6Am+eW5bSEIbPrFEqvoBvx7e1alzAorKWXYHMETErE56j+AStAJ/WPA==;EndpointSuffix=core.windows.net";

// Récupération du Blob Service associé au Storage account
BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);
string containerName="scripts";

// -------------------Création d'un container----------------------
BlobContainerClient blobContainerClient=await blobServiceClient.CreateBlobContainerAsync(containerName);

Console.WriteLine($"Container [{blobContainerClient.Name}] created");
//-----------------------------------------------------------------

//----------------------Upload d'un fichier-----------------------
string containerName="scripts";
string fileName="01.sql";
string path=@"C:\tmp8\01.sql";
BlobContainerClient blobContainerClient=blobServiceClient.GetBlobContainerClient(containerName);

BlobClient blobClient=blobContainerClient.GetBlobClient(fileName);

await blobClient.UploadAsync(path);

Console.WriteLine("Uploaded Blob to container");
//-----------------------------------------------------------------

//------------------Lister les blobs d'un container-------------------
string containerName="images";
BlobContainerClient blobContainerClient=blobServiceClient.GetBlobContainerClient(containerName);

await foreach(BlobItem blobItem in blobContainerClient.GetBlobsAsync())
{
    Console.WriteLine($"Blob name {blobItem.Name}");
    Console.WriteLine($"Blob size {blobItem.Properties.ContentLength}");
}
//-----------------------------------------------------------------

//----------------------Download d'un fichier-----------------------
string containerName="scripts";
string fileName="01.sql";
string path=@"C:\tmp4\01.sql";

BlobContainerClient blobContainerClient=blobServiceClient.GetBlobContainerClient(containerName);
BlobClient blobClient=blobContainerClient.GetBlobClient(fileName);

await blobClient.DownloadToAsync(path);

Console.WriteLine("Download operation is complete");
//-----------------------------------------------------------------

//------------------Gestion des métadata des blobs-------------------
string containerName="scripts";
string blobName="01.sql";

BlobContainerClient blobContainerClient = blobServiceClient.GetBlobContainerClient(containerName);

blobMetaData = new Dictionary<string,string>();
blobMetaData.Add("Department","Human Resources");

BlobClient blobClient=blobContainerClient.GetBlobClient(blobName);

await blobClient.SetMetadataAsync(blobMetaData);

BlobProperties blobProperties = await blobClient.GetPropertiesAsync();
foreach(var metaData in blobProperties.Metadata)
{
    Console.WriteLine($"Key - {metaData.Key}");
    Console.WriteLine($"Value - {metaData.Value}");
}
```

**Note** : Ici on a utilisé la **connection string** de la ressource Azure Blob Storage, ce qui donne à l'application un **accès total** au Blob Storage.
Pour une **gestion** plus **fine** des **autorisations** données à l'application, il convient de créer un [[Azure Application Object|application object]], qui représentera l'application en tant que [[Security principals|security principal]].

Une fois cet application object créé et configuré (création du Client Secret), il convient de télécharger le package ``Azure.Identity`` : 
```dotnet
dotnet add package Azure.Identity
```

Puis d'utiliser la classe ``ClientSecretCredentials`` pour instancier un BlobClient (ici, exemple de téléchargement de blob après configuration de droits d'écriture via l'Application Object).
```csharp
using Azure.Identity;
using Azure.Storage.Blobs;

string containerName="data";
string fileName="script01.ps1";
string path=@"C:\tmp4\script01.ps1";

// Identifiant Microsoft Entra (tenant, ou directory)
string tenantId="38dbefc3-d57f-4955-b62c-1406e16a4ea8";
// Identifiant de l'Application object
string clientId="72ee1803-08cf-4c19-a334-8405e09a242c";
// Value du Client Secret créé au sein de l'Application Object
string secret="ywo8Q~iDGv1b1.Uk_CkXbLUw2G4YNLbWUo5s4b1T";
string storageAccountName="appstore55455344243";
// Construction de l'URI du blob - schéma par défaut
string blobUri=$"https://{storageAccountName}.blob.core.windows.net/{containerName}/{fileName}";

ClientSecretCredential clientSecretCredential = new ClientSecretCredential(tenantId,clientId,secret);

BlobClient blobClient= new BlobClient(new Uri(blobUri),clientSecretCredential);

await blobClient.DownloadToAsync(path);
```