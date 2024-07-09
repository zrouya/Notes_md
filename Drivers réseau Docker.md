
Un driver réseau Docker est un **plugin** ou une technologie qui permet de **gérer les réseaux** de conteneurs [[Docker]].
Ces drivers définissent les règles et la manière dont la communication réseau se déroule dans l'environnement Docker.

Il existe plusieurs types de drivers réseau dans Docker, chacun adapté à des besoins spécifiques :

1. **Bridge**: C'est le driver par défaut lorsque vous créez un réseau Docker. Il crée un réseau interne privé auquel les conteneurs peuvent se connecter, leur permettant de communiquer entre eux. Le bridge est isolé du réseau hôte par défaut, mais vous pouvez configurer la communication entre le réseau hôte et le bridge.
    
2. **Host**: Ce driver retire l'isolation entre les conteneurs Docker et le système hôte, en attachant directement le conteneur au réseau de l'hôte. Cela signifie que les conteneurs partagent l'espace de noms réseau de l'hôte.
    
3. **[[Réseaux Docker Overlay|Overlay]]**: Ce driver permet de connecter plusieurs daemons Docker entre eux, permettant ainsi aux conteneurs exécutés sur des hôtes Docker différents de communiquer entre eux. Il est souvent utilisé dans des environnements [[Docker Swarm]] pour gérer la communication entre les conteneurs sur différents hôtes.
    
4. **Macvlan**: Permet aux conteneurs de paraître comme des périphériques physiques sur le réseau, avec leur propre adresse MAC. Utile dans les cas où vous avez besoin que les conteneurs soient directement accessibles dans le réseau sans passer par le routage NAT du hôte Docker.
    
5. **None**: Aucun réseau n'est attaché à un conteneur utilisant ce driver. Cela peut être utile pour des conteneurs qui doivent être complètement isolés ou pour des cas d'utilisation très spécifiques où vous gérez manuellement le réseau.
