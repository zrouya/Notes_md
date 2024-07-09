
Azure App Configuration est une [[Resource Azure|ressource]] Azure permettant de mettre à disposition des **paires clé/valeur** à une [[Azure Web Application|application]] Azure.

Pour une application [[ASP.NET Core|ASP .Net Core]], il est possible, via le package Nuget Microsoft.Extensions.Configuration.**AzureAppConfiguration**, d'accéder à la Config Azure via sa **connection string** (voir la section "**Settings/Access keys**" de la ressource App Configuration).
Dans le code de build de l'application, utiliser la méthode ``ConfigureAppConfiguration``  pour lier la configuration de l'application à la Azure App Configuration : 

![[Pasted image 20240318173243.png]]

Une fois la configuration liée, elle peut être accessible au sein d'une application Web via les objets injectés de type *IConfiguration* : 

![[Pasted image 20240318173539.png]]