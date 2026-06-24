---
tags: [tls, https, reseau, securite]
---

# Connexion HTTPS — déroulé complet

De `https://example.com` tapé dans le navigateur jusqu'à l'affichage de la page.

## Les étapes

1. **DNS** : résolution `example.com` → adresse IP
2. **TCP** : 3-way handshake (SYN / SYN-ACK / ACK) sur le port **443**
3. **TLS handshake** :
   - négociation version + cipher suite
   - le serveur présente sa [[chaine-de-certificats]]
   - le client la valide (signatures, dates, SAN, révocation)
   - échange [[diffie-hellman-ephemere|ECDHE]] → dérivation des clés symétriques
   - négociation **ALPN** (HTTP/1.1 vs HTTP/2)
4. **Canal sécurisé établi** : tout est désormais chiffré en symétrique (AES-GCM / ChaCha20)
5. **Requête HTTP** : `GET / HTTP/2`, en-têtes, cookies — dans le tunnel chiffré
6. **Réponse HTTP** : le serveur renvoie le HTML, chiffré
7. **Réutilisation** : connexion gardée ouverte (keep-alive) ou [[reprise-session-0-rtt|reprise de session]] pour éviter un nouveau handshake complet

## Schéma mental

```
DNS → TCP → TLS handshake → [tunnel chiffré] → HTTP req/resp
```

## Voir aussi

- [[https-vue-ensemble]]
- [[handshake-tls-1-2]]
- [[handshake-tls-1-3]]
- [[reprise-session-0-rtt]]
