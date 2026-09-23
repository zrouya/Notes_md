

Tout comme les [[Hub réseau|hubs]], les switches permettent de connecter plusieurs membres d'un réseau, en assurant le rôle de **noeud central** (voir [[Topologie réseau en étoile|réseau en étoile]]).

Ils sont considérés comme faisant partie de la [[Couche de liaison de données du modèle OSI|couche 2]] du modèle OSI (**smart device**).

Ils ont la capacité de **mémoriser** les [[Adresses MAC|adresses MAC]] des appareils connectés à lui, via une **Table d'adresses MAC (MAC addresses table)**, également appelée **Content Addressable Memory (CAM) table**. Les paquets ne sont alors transmis qu'aux ports de destination, même si le broadcast reste possible.

Ce procédé de mise en mémoire d'adresses MAC s'effectue grâce aux [[Application-Specific Integrated Circuitery (ASIC)|puces ASIC]]. 

Un switch va donc connaitre les **adresses MAC source et destination** lors des processus de communication, ce qui **réduit** les **risques de collision** réseau.

![[Pasted image 20240222144059.png]]