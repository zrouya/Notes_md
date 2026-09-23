
Les routeurs et les tables de routage jouent un rôle crucial dans la gestion du trafic sur les réseaux informatiques. Ils sont responsables de l'acheminement des paquets de données à travers les réseaux en utilisant des tables de routage. 
### Routeurs

1. **Fonction Principale** :
    
    - Un routeur est un dispositif qui connecte plusieurs réseaux et dirige le trafic entre eux. Il examine l'en-tête des paquets de données, notamment l'adresse IP de destination, et détermine le meilleur chemin pour acheminer chaque paquet vers sa destination.

2. **Acheminement des Paquets** :
    
    - Les routeurs utilisent des algorithmes pour déterminer le chemin le plus efficace pour acheminer les paquets. Ils peuvent changer de route si nécessaire, par exemple, en cas de congestion ou de panne d'un lien réseau.

### Tables de Routage

1. **Rôle** :
    
    - Une table de routage est une liste d'itinéraires enregistrée dans un routeur. Elle contient des informations sur les chemins à prendre pour atteindre différentes destinations réseau.

2. **Contenu** :
    
    - Une table de routage typique inclut l'adresse de destination, le [[Masque de sous-réseau|masque de sous-réseau]], la [[Passerelle réseau|passerelle]], l'[[Interface réseau|interface]] à utiliser, et parfois la distance ou le coût du chemin.