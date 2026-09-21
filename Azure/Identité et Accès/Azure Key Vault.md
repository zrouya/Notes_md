
Azure Key Vault est une [[Resource Azure|ressource Azure]] permettant de stocker des données de manière sécurisée :
- secrets applicatifs (connection strings, ...)
- clés de cryptage
- certificats TLS

Une fois la ressource créée, on peut ajouter des secret, des encryption keys, et des certificats :
![[Pasted image 20240815105828.png]]

**Note** : Même avec le Azure Admin account ayant créé la ressource, il est nécessaire d'**assigner les rôles** permettant de gérér la ressource : ex : ``Key Vault Administrator``.
(**Access control (IAM) / Role assignments**).

## Utilisation des clés de chiffrement

Pour qu'une application puisse utiliser une clé de chiffrement : 
- Créer un [[Azure Application Object]]
- Créer un [[Azure Application Object#Creation d'un client secret|Client Secret]]
- Associer les bons **[[Rôles Azure RBAC (Role Based Access Control)|rôles]]** à l'application object, par exemple ``Key Vault Crypto User`` (à partir de la ressource **Key Vault**, ou de l'objet **Key**, depuis **Access control (IAM)**)

Pour une application .Net, ajouter les packages ``Azure.Identity`` et ``Azure.Security.KeyVault.Keys``, puis utiliser la classe ``KeyClient`` pour accéder à la clé :

```csharp
using System.Text;
using Azure.Identity;
using Azure.Security.KeyVault.Keys;
using Azure.Security.KeyVault.Keys.Cryptography;

// Credentials Client secret
string clientId="da92e330-5968-4593-b3b5-f070fd6aaeee";
string tenantId="70c0f6d9-7f3b-4425-a6b6-09b47643ec58";
string clientSecret="9Hi8Q~t.0skdyQR-.aP9.4TSdpjDVWUZ4hDiecj-";
// URI de la ressource Azure Key Vault
string keyVaultURI="https://appvault2939949.vault.azure.net/";
string keyName="appkey";

ClientSecretCredential clientSecretCredential=new ClientSecretCredential(tenantId, clientId,clientSecret);

KeyClient keyClient = new KeyClient(new Uri(keyVaultURI),clientSecretCredential);

var key=keyClient.GetKey(keyName);
Console.WriteLine($"Got a handle to the key {key.Value.Name}");

// Next we need to use the CryptographyClient class to perform cryptographic operations

string toEncrypt="This is a string we want to encrypt";
var cryptoClient = keyClient.GetCryptographyClient(key.Value.Name, key.Value.Properties.Version);

byte[] plainText = Encoding.UTF8.GetBytes(toEncrypt);

EncryptResult encryptResult = cryptoClient.Encrypt(EncryptionAlgorithm.RsaOaep, plainText);

Console.WriteLine($"Encrypted text {Convert.ToBase64String(encryptResult.Ciphertext)}");

// If you want to decrypt the text

byte[] ciperToBytes = encryptResult.Ciphertext;
var decryptedText=cryptoClient.Decrypt(EncryptionAlgorithm.RsaOaep,encryptResult.Ciphertext);

Console.WriteLine(Encoding.UTF8.GetString(decryptedText.Plaintext));
``` 

## Utilisation d'un secret

Les premières étapes pour l'utilisation d'un secret sont les mêmes que pour l'utilisation d'une [[Azure Key Vault#Utilisation des clés de chiffrement|clé]].

Pour une application .Net, ajouter les packages ``Azure.Identity`` et ``Azure.Security.KeyVault.Secrets``, puis utiliser la classe ``SecretClient`` :

```csharp
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

string clientId="da92e330-5968-4593-b3b5-f070fd6aaeee";
string tenantId="70c0f6d9-7f3b-4425-a6b6-09b47643ec58";
string clientSecret="9Hi8Q~t.0skdyQR-.aP9.4TSdpjDVWUZ4hDiecj-";
string keyVaultURI="https://appvault2939949.vault.azure.net/";

string secretName="appsecret";

ClientSecretCredential clientSecretCredential=new ClientSecretCredential(tenantId, clientId,clientSecret);

SecretClient secretClient=new SecretClient(new Uri(keyVaultURI),clientSecretCredential);

var secret=secretClient.GetSecret(secretName);

Console.WriteLine($"Secret Value - {secret.Value.Value}");
```
