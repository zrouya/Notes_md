
Les Azure Managed Identities sont un moyen d'attribuer des statuts de [[Security principals|security principals]] à des [[Resource Azure|ressources]] Azure, comme des [[Machines virtuelles Azure|Azure VMs]], des [[Azure Functions|Azure functions]], des [[Azure Web Application|Azure web app]], etc...

Elles permettent aux ressources hébergées par Azure de s'**authentifier**, et d'utiliser des **autorisations** basées sur des [[Rôles Azure RBAC (Role Based Access Control)|rôles]], **sans** avoir besoin de **stocker** des **credentials** d'authentification.

Il existe 2 types de Managed Identity :
- **System assigned** : Managed Identity créée et affectée automatiquement par Azure
	Elle ne peut concerner **qu'une seule ressource** et a le même **cycle de vie** que celle-ci (elle est supprimée quand la ressource est supprimée)
- **User assigned** : Managed Identity créée par un utilisateur, il s'agit d'une **ressource en elle-même** et a donc un **cycle de vie indépendant**. Elle peut être appliquée à **plusieurs ressources**.

## Création d'une Managed Identity
### System assigned Managed Identity
Exemple pour une VM : 
![[Pasted image 20240815163935.png]]

Une fois la Managed Identity créée pour la ressource, celle ci est sélectionnable en tant que **principal** pour lui affecter des **rôles**.

Via [[Azure PowerShell]] : 
```powershell
Connect-AzAccount
$vmName="appvm"
$resourceGroupName="app-grp"

$vm = Get-AzVM -ResourceGroupName "app-grp" -Name $vmName
Update-AzVM -ResourceGroupName "app-grp" -VM $vm -IdentityType SystemAssigned
```
## Fonctionnement

Lorsqu'une ressource Azure ayant une Managed Identity veut accéder à une autre ressource Azure, elle obtient d'abord un **access token** auprès du service **Azure IMDS (Instance Metadata Service)** (en effectuant une requête HTTP).

Ce service est accessible par n'importe quelle ressource Azure via une **adresse IP "link-local"** fixe : **169.254.169.254**. La requête est ensuite routée pour qu'IMDS renvoie les données de manière **sécurisée** et **isolée** pour chaque ressource.

La requête prend en **query parameter** l'URI de la ressource à laquelle on veut accéder, par exemple ``resource=https://storage.azure.com/`` ou ``resource=https://management.azure.com/``.

Voir [la documentation](https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/tutorial-windows-vm-access?pivots=windows-vm-access-wvm)

## Applications .Net

Au sein d'une application .Net, à partir d'une ressource ayant une Managed Identity, il n'est plus utile d'instancier un [[Azure Application Object#Creation d'un client secret|ClientSecretCredentials]], à la place un ``DefaultAzureCredential`` suffit :
```csharp
using Azure.Identity;
using Azure.Storage.Blobs;

string containerName = "data";
string fileName = "script01.ps1";
string path = @"C:\tmp4\script01.ps1";

string storageAccountName = "appstore55455344243";
string blobUri = $"https://{storageAccountName}.blob.core.windows.net/{containerName}/{fileName}";

BlobClient blobClient = new BlobClient(new Uri(blobUri),new DefaultAzureCredential());

await blobClient.DownloadToAsync(path);
```

En passant un DefaultAzureCredential, Azure va **parcourir** différentes **méthodes d'authentification**, et choisira la première qui fonctionne (dont celle reposant sur les Managed Identity).

L'appel à IMDS est transparent, le code est équivalent à : 

```csharp
using System.Net.Http.Headers;
using System.Text.Json;

string tokenUri = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://storage.azure.com/";
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Add("Metadata","true");

HttpResponseMessage responseMessage = await httpClient.GetAsync(tokenUri);

string content = await responseMessage.Content.ReadAsStringAsync();

Dictionary<string,string>? values = JsonSerializer.Deserialize<Dictionary<string,string>>(content);

string containerName = "data";
string fileName = "script01.ps1";

string storageAccountName = "appstore55455344243";
string blobUri = $"https://{storageAccountName}.blob.core.windows.net/{containerName}/{fileName}";

HttpClient client =  new HttpClient();
client.DefaultRequestHeaders.Add("x-ms-version","2024-05-04");
client.DefaultRequestHeaders.Authorization =  new AuthenticationHeaderValue("Bearer",values["access_token"]);

HttpResponseMessage message = await client.GetAsync(blobUri);
string blobContent = await message.Content.ReadAsStringAsync();

Console.WriteLine(blobContent);
```