# Azure — Map of Content

## API Management (APIM)

- [[apim-rate-limiting]] — 4 niveaux de granularité, familles rate-limit vs quota, cumul et caveat multi-instance
- [[apim-caching]] — Vue d'ensemble du cache APIM : mode interne vs externe (Redis)
- [[apim-cache-policies]] — Politiques cache-lookup, cache-store, cache-remove-value et leurs paramètres
- [[apim-cache-bonnes-pratiques]] — Bonnes pratiques, limites, et approche ETag manuelle

## Monitoring & Observability

- [[appinsights-principe]] — Principe général d'Application Insights : connection string, flux de données, SDK vs codeless
- [[appinsights-auto-instrumentation]] — Auto-instrumentation codeless sur Azure Container Apps (.NET et Java sans modifier le code)

## Identity & IAM

- [[managed-identity-system-assigned]] — Identité liée au cycle de vie d'une ressource, non partageable
- [[managed-identity-user-assigned]] — Identité indépendante, attachable à plusieurs ressources
- [[managed-identity-comparaison]] — Tableau comparatif System vs User Assigned
- [[entra-graph-permissions]] — Permissions Microsoft Graph : délégué (`scp`) vs application (`roles`), consentement
- [[entra-directory-roles]] — RBAC sur l'annuaire : actions `microsoft.directory/*`, scoping par AU, PIM
- [[entra-permissions-vs-roles]] — Différences conceptuelles des deux référentiels, recouvrement et règle de choix
- [[entra-app-registration-vs-enterprise-app]] — Objets `application` vs `servicePrincipal`, relation 1→N, cas des managed identities
- [[entra-audit-permissions-sp]] — Vérifier les permissions réelles d'un compte de service (portail + CLI), pièges `Group.Create`
