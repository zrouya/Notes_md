---
tags: [tls, https, securite]
---

# Sécurité HTTPS — HSTS, Pinning, MITM

Mécanismes de durcissement et points d'attention autour de TLS.

## MITM (Man-In-The-Middle)

- Un attaquant (ou proxy d'inspection) ne peut intercepter HTTPS qu'en présentant **son propre certificat**
- Ce certificat n'est accepté que si **sa CA est dans le trust store** du client
- ➜ Cas des **proxys d'entreprise** : leur CA est installée sur les postes, ce qui leur permet de déchiffrer le trafic légitimement

## HSTS — HTTP Strict Transport Security

- En-tête `Strict-Transport-Security: max-age=31536000; includeSubDomains`
- Force le navigateur à **n'utiliser que HTTPS** pour ce domaine
- Empêche le **downgrade** vers HTTP (attaque SSL stripping)

## Certificate Pinning

- L'application (souvent mobile) **épingle** la clé publique attendue
- Refuse tout autre certificat, **même valablement signé par une CA**
- Protection contre une **CA compromise** ou un proxy MITM
- ⚠️ Risque de casse si le certificat tourne sans mise à jour de l'app

## Pièges de validation fréquents

- Chaîne **intermédiaire manquante** → voir [[chaine-de-certificats]]
- Certificat **expiré** (dates `Not After`)
- **Mismatch** SNI / SAN
- **Horloge client** désynchronisée → échec de la validation des dates

## Voir aussi

- [[chaine-de-certificats]]
- [[autorite-certification]]
- [[certificat-x509]]
