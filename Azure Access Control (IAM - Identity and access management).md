
Le Azure Access Control est le service de gestion des **utilisateurs** et des **autorisations** associées.

L'access control peut être défini : 
- Au niveau d'une [[Azure Subscription|subscription]] Azure entière
- Au niveau d'un [[Resource group Azure|resource group]]
- Au niveau d'une [[Resource Azure|ressource]] Azure unique

La gestion est basée sur des **rôles** (**Role Based Access Control - RBAC**), qui peuvent s'appuyer sur des rôles [[Microsoft Entra ID (ex. Azure Active Directory)|Microsoft Entra]] :

![[Pasted image 20240814171910.png]]

Il existe une **centaine** de rôle Azure **prédéfinis** (**built-in**), dont les 5 fondamentaux : 
- **Owner** 
- **Contributor**
- **Reader**
- **Role Based Access Control Admin**
- **User Access Admin**

Voir [la documentation](https://learn.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles) des rôle Azure et de leur gestion.