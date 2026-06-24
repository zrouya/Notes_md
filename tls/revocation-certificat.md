---
tags: [tls, certificat, revocation, securite]
---

# Révocation de certificat

Un certificat peut être compromis **avant** son expiration. Deux mécanismes pour le signaler.

## CRL — Certificate Revocation List

- Grande liste de numéros de série révoqués, publiée par la CA
- Lourd à télécharger et tenir à jour

## OCSP — Online Certificate Status Protocol

- Le client interroge la CA pour **un certificat précis** ("est-il révoqué ?")
- Problèmes : latence ajoutée + fuite de vie privée (la CA voit les sites visités)

## OCSP Stapling (la solution moderne)

- C'est le **serveur** qui joint une preuve OCSP **fraîche et signée par la CA** directement dans le handshake
- Le client n'a plus à contacter la CA → rapide et privé

```bash
# Vérifier le stapling d'un serveur
openssl s_client -connect example.com:443 -status </dev/null 2>/dev/null \
  | grep -A 17 "OCSP response"
```

## Voir aussi

- [[chaine-de-certificats]]
- [[certificat-x509]]
- [[openssl]]
