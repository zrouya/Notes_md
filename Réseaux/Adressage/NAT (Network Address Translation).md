
1. **Fonction du NAT** :
    
    - NAT est une méthode utilisée pour mapper des adresses IP privées à une adresse IP publique et vice versa. Cela est souvent utilisé dans les réseaux domestiques et d'entreprise où de nombreux dispositifs internes partagent une adresse IP publique unique pour accéder à Internet.

2. **Fonctionnement** :
    
    - Quand un dispositif d'un réseau privé accède à Internet, le [[Routage réseau|routeur]] remplace l'adresse IP privée source dans les paquets sortants par son adresse IP publique. Pour le trafic entrant, le routeur utilise des informations de mappage pour déterminer à quel dispositif interne le paquet est destiné.

3. **Utilité** :
    
    - NAT aide à économiser les adresses IP publiques et ajoute une couche de sécurité, car les adresses IP des dispositifs internes ne sont pas exposées directement sur Internet.