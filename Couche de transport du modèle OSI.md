
La couche de transport du modèle [[Modèle OSI|OSI]] (Transport Layer) constitue la **couche 4** du modèle.

Le niveau de granularité des données est le [[Segments (couche transport)|segment]] : les données à transporter sont **découpées en segments**, qui doivent être **réassemblés** pour que le destinataire reforme la donnée envoyée.

Le but de la couche transport est de s'assurer de l'**intégrité des segments** qui transitent sur le réseau, de leur transmission **dans le bon ordre**, et du **bon réassemblage** des segments chez le destinataire.

Les protocoles utilisés peuvent être :
- **Orientés Connexion** ([[Protocole TCP|TCP]]) : L'expéditeur et le destinataire opèrent un **handshake**, et l'**expéditeur** est **notifié** de la **bonne réception** des segments.
- **Sans Connexion** ([[Protocole UDP|UDP]]) : **Pas** de **handshake**, **pas** de **notification** de bonne réception.

La couche transport est également responsable de deux aspects du contrôle de flux de données :
- Le **Buffering** : les **segments** à envoyer sont **mis en mémoire** du côté de l'**expéditeur**, **en attendant** que le **destinataire** puisse les **recevoir**.
- Le **Windowing** : Expéditeur et destinataire **se mettent d'accord** sur la **taille** optimale des **segments** à employer pour le transfert de données.

Elle encapsule les données de la couche 5 avec les **numéros de ports** source et destination.