---
tags: [azure, app-service, dns]
---

# Domain names Azure Web App

Azure permet de gérer les **noms de domaines** relatifs à une [[azure-web-application|Web app]] Azure. Il est par exemple possible d'ajouter un nom de domaine à une application, via la section **Settings/Custom domains** du portail Azure.

## Enregistrements DNS requis

Il suffit pour cela d'ajouter les [[DNS record|enregistrements DNS]] (DNS records) correspondants :
- Un record de type A ou AAAA, pour spécifier l'adresse IP correspondante
- Un record de type TXT, de nom **asuid** (Token fourni par Azure), qui permet à Azure de vérifier que le nom de domaine est bien détenu par l'organisation configurant l'application.

![[Pasted image 20240314092655.png]]

## Voir aussi

- [[azure-web-application]]
- [[certificat-ssl-azure-web-app]]
