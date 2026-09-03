---
tags: [reseau, ethernet, couche2]
---

# CSMA/CD — Collision Detection

Méthode d'accès au support d'[[Ethernet]] filaire, en [[Couche de liaison de données du modèle OSI|couche 2]]. Conçue pour un support **partagé** où plusieurs machines peuvent émettre en même temps.

## Le principe, en trois temps

1. **Carrier Sense** — écouter avant d'émettre : le support est-il libre ?
2. **Multiple Access** — plusieurs machines partagent le même support
3. **Collision Detection** — continuer d'écouter **pendant** l'émission ; si le signal lu diffère du signal émis, il y a collision

En cas de collision : émission d'un signal de bourrage (*jam*), puis attente d'un délai aléatoire avant nouvelle tentative — le *backoff exponentiel binaire*, qui double la fenêtre d'attente à chaque échec successif.

## Pourquoi c'est devenu obsolète

CSMA/CD suppose un **domaine de collision partagé** : c'était le cas avec les câbles coaxiaux et les [[Hub réseau|hubs]], qui répètent le signal sur tous les ports.

Un [[Switch réseau|switch]] change tout : chaque port est son propre domaine de collision, et le mode **full-duplex** permet d'émettre et recevoir simultanément sur des paires distinctes. Il n'y a plus de collision possible.

> Sur un réseau Ethernet commuté moderne, CSMA/CD est désactivé. Le mécanisme reste dans la norme pour la compatibilité half-duplex.

## À ne pas confondre

Le Wi-Fi ne peut pas détecter les collisions — une radio ne peut pas écouter et émettre en même temps sur la même fréquence. Il les **évite** en amont : voir [[Carrier Sense Multiple Access - Collision Avoidance (CSMA-CA)|CSMA/CA]].

## Voir aussi

- [[Ethernet]] · [[Hub réseau]] · [[Switch réseau]]
- [[Couche de liaison de données du modèle OSI]]
- [[Frames (couche liaison de données)]]
