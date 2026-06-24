---
tags: [tls, certificat, ca, securite]
---

# Autorité de certification (CA)

Tierce partie de confiance qui **atteste** qu'une clé publique appartient bien à un domaine, en **signant** son certificat avec sa propre clé privée.

## Le problème résolu

> Comment faire confiance à une clé publique reçue sur un réseau hostile ?

Réponse : une autorité reconnue garantit le lien identité ↔ clé publique.

## Trust store

- L'OS et le navigateur embarquent une liste de **CA racines** de confiance par défaut
- Exemples : DigiCert, Let's Encrypt / ISRG, GlobalSign
- C'est l'**ancre de confiance** : tout certificat doit remonter à une racine de ce store

## Pourquoi des intermédiaires ?

- Les clés des **CA racines** sont trop précieuses → gardées **hors-ligne** dans des HSM
- Elles ne signent pas les certificats de sites directement
- Elles signent des **CA intermédiaires**, qui signent les certificats feuilles → voir [[chaine-de-certificats]]

## Voir aussi

- [[chaine-de-certificats]]
- [[certificat-x509]]
- [[securite-https-hsts-pinning-mitm]]
