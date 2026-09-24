---
tags: [architecture, event-sourcing, rgpd, securite]
---

# Crypto-shredding

Rendre des données définitivement illisibles en **détruisant leur clé de chiffrement** plutôt que les données elles-mêmes. C'est la réponse classique au droit à l'oubli (RGPD) dans un store immuable.

## Principe

```
Événement stocké : { ClientId: 7, Nom: <chiffré avec clé-7>, Montant: 200 }
Coffre de clés   : clé-7

Demande d'effacement → suppression de clé-7
→ Nom devient illisible à jamais, le Montant reste exploitable
```

## Détails

- **Une clé par personne** (sujet des données), stockée hors de l'event store (Key Vault, table dédiée).
- Ne chiffrer que les champs personnels : les données métier non personnelles restent lisibles.
- À la lecture, si la clé est absente, on remplace par une valeur neutre (`"<effacé>"`).
- Penser aussi aux **projections**, aux snapshots et aux sauvegardes qui contiennent la donnée en clair.
- Alternative : ne pas mettre de données personnelles dans les événements, seulement une référence vers un store classique qu'on peut supprimer.

## Voir aussi

- [[event-sourcing]]
- [[event-store]]
