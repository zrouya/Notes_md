
L'encodage est un processus consistant à convertir des données d'un format à un autre. Les informations peuvent être encodées pour de nombreuses raisons, telles que la standardisation, la compression, le cryptage ou la préparation pour le transport.

## **Encodage de Caractères**

- **ASCII** : l'un des plus anciens systèmes d'encodage de caractères. Il utilise 7 bits pour représenter 128 caractères différents.
- **Unicode :** Unicode est un système d'encodage conçu pour représenter presque tous les caractères de toutes les langues écrites dans le monde. Il attribue un numéro unique à chaque caractère, quelle que soit la plateforme, le programme ou la langue.
- **UTF-8** : un système d'encodage plus moderne et flexible qui peut représenter tous les caractères dans le jeu universel de caractères Unicode. UTF-8 est compatible avec ASCII (il code les caractères ASCII sur 1 octet également) mais peut utiliser jusqu'à 4 octets pour représenter un caractère.
- **UTF-16** : Variante du UTF-8, qui code les caractères sur 2 à 4 octets. Il est plus efficace que le UTF-8 pour les langues asiatiques. Il est également utilisé par de nombreux systèmes Windows pour le stockage interne des chaînes de caractères.

## **Stockage des Données**

- Les données sont stockées dans des unités appelées bits, qui peuvent être 0 ou 1.
- Les bits sont généralement regroupés en unités plus grandes appelées octets, qui contiennent 8 bits.
- Un ordinateur avec une architecture de 64 bits peut lire 64 bits (ou 8 octets) de données à la fois.
- Chaque octet de la mémoire de l'ordinateur a sa propre adresse unique. Le système sait combien d'octets il doit lire en se basant sur le type de la donnée.

## **Encodage Base64 et Base64Url**

- **Base64** est un système d'encodage qui représente les données binaires en chaînes de caractères ASCII. **Il traite les données par groupe de 3 octets (24 bits).**
- Si le nombre total d'octets n'est pas un multiple de 3, les octets supplémentaires sont remplis de zéros pour former un groupe complet de 3 octets.
- Il **divise chaque groupe en 4 unités de 6 bits**, et encode chaque **unité en un caractère ASCII**. Les caractère ASCII utilisés pour cet encodage sont principalement des chiffres, des lettres majuscules et minuscules (ce qui donne 62 caractères) et deux autres caractères, généralement "+" et "/".
- Si le nombre d'octets d'entrée n'était pas un multiple de 3, des caractères de padding "=" sont ajoutés à la fin de la chaîne de caractères de sortie pour la rendre d'une longueur multiple de 4.
- **Base64Url** est une variante de Base64 qui est sûre à utiliser dans les URL et les cookies. Il remplace les caractères "+" et "/" par "-" et " _ ", et supprime les caractères "=" de padding à la fin de la chaîne encodée.