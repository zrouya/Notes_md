---
tags: [outils, encodage, unicode]
---

# Encodage

L'encodage est un processus consistant à convertir des données d'un format à un autre. Les informations peuvent être encodées pour de nombreuses raisons : standardisation, compression, cryptage ou préparation pour le transport.

## Encodage de caractères

- **ASCII** : l'un des plus anciens systèmes d'encodage de caractères. Il utilise 7 bits pour représenter 128 caractères différents.
- **Unicode** : système d'encodage conçu pour représenter presque tous les caractères de toutes les langues écrites dans le monde. Il attribue un numéro unique à chaque caractère, quelle que soit la plateforme, le programme ou la langue.
- **UTF-8** : système d'encodage moderne et flexible qui peut représenter tous les caractères du jeu universel Unicode. Compatible avec ASCII (il code les caractères ASCII sur 1 octet également) mais peut utiliser jusqu'à 4 octets par caractère.
- **UTF-16** : variante qui code les caractères sur 2 à 4 octets. Plus efficace que l'UTF-8 pour les langues asiatiques ; utilisé par de nombreux systèmes Windows pour le stockage interne des chaînes de caractères.

## Stockage des données

- Les données sont stockées dans des unités appelées bits, qui peuvent être 0 ou 1.
- Les bits sont regroupés en octets (8 bits).
- Un ordinateur avec une architecture 64 bits peut lire 64 bits (8 octets) de données à la fois.
- Chaque octet de la mémoire a sa propre adresse unique ; le système sait combien d'octets lire en se basant sur le type de la donnée.

## Encodage Base64 et Base64Url

- **Base64** représente des données binaires en chaînes de caractères ASCII. Il traite les données par groupes de 3 octets (24 bits).
- Si le nombre total d'octets n'est pas un multiple de 3, les octets supplémentaires sont remplis de zéros pour former un groupe complet.
- Chaque groupe est divisé en 4 unités de 6 bits, chaque unité étant encodée en un caractère ASCII (chiffres, lettres majuscules/minuscules, plus généralement "+" et "/", soit 64 caractères).
- Si le nombre d'octets d'entrée n'était pas un multiple de 3, des caractères de padding "=" sont ajoutés à la fin pour obtenir une longueur multiple de 4.
- **Base64Url** est une variante sûre pour les URL et cookies : elle remplace "+" et "/" par "-" et "_", et supprime le padding "=".

## Voir aussi

- [[_index/outils|Index Outils]]
