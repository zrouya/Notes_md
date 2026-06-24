---
tags: [tls, certificat, ca, securite]
---

# Chaîne de certificats

Suite de certificats qui relie le certificat d'un site à une **CA racine** de confiance.

## Structure

```
Root CA            ← dans le trust store de l'OS/navigateur, clé hors-ligne
  │ signe
Intermediate CA    ← envoyée par le serveur pendant le handshake
  │ signe
Leaf / End-entity  ← le certificat de example.com (clé publique du serveur)
```

## Validation côté client

1. Part du certificat **feuille** (`example.com`)
2. Vérifie sa signature avec la clé publique de l'**intermédiaire**
3. Vérifie la signature de l'intermédiaire avec la clé publique de la **racine**
4. Vérifie que cette racine est dans le **trust store** → ancre de confiance ✅

À chaque maillon, on vérifie aussi :
- les **dates de validité**
- la correspondance domaine ↔ **SAN** du certificat feuille
- la **non-révocation** ([[revocation-certificat]])
- les contraintes (`Basic Constraints: CA:TRUE` pour un intermédiaire)

## ⚠️ Piège classique : intermédiaire manquant

Le serveur doit servir la **chaîne complète sauf la racine** (inutile, déjà chez le client).
Oubli fréquent → erreur `unable to verify the first certificate` (curl, Java, mobiles).

## Voir aussi

- [[autorite-certification]]
- [[certificat-x509]]
- [[revocation-certificat]]
- [[openssl]]
