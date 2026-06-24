---
tags: [outils, tls, certificat, cli, securite]
---

# openssl — Inspection TLS / certificats

Boîte à outils en ligne de commande pour inspecter handshakes, certificats et chaînes.

## Inspecter un handshake et la chaîne servie

```bash
# Connexion + détail de la chaîne présentée par le serveur
openssl s_client -connect example.com:443 -servername example.com </dev/null

# Afficher uniquement la chaîne de certificats
openssl s_client -connect example.com:443 -servername example.com \
  -showcerts </dev/null 2>/dev/null

# Forcer une version TLS
openssl s_client -connect example.com:443 -tls1_3 </dev/null
openssl s_client -connect example.com:443 -tls1_2 </dev/null
```

> `-servername` envoie le **SNI** — indispensable en hébergement mutualisé.

## Décoder un certificat

```bash
# Lire un certificat fichier
openssl x509 -in cert.pem -noout -text

# Récupérer + décoder le certificat d'un site
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null \
  | openssl x509 -noout -text

# Champs ciblés : dates, SAN, issuer
openssl x509 -in cert.pem -noout -dates
openssl x509 -in cert.pem -noout -ext subjectAltName
openssl x509 -in cert.pem -noout -issuer -subject
```

## Vérifier une chaîne / OCSP

```bash
# Valider une chaîne contre un bundle de CA
openssl verify -CAfile chain.pem cert.pem

# Vérifier le stapling OCSP
openssl s_client -connect example.com:443 -status </dev/null 2>/dev/null \
  | grep -A 17 "OCSP response"
```

## Voir aussi

- [[certificat-x509]]
- [[chaine-de-certificats]]
- [[revocation-certificat]]
- [[handshake-tls-1-2]]
