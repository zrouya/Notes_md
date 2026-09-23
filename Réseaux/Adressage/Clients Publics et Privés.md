
En sécurité informatique, notamment dans le cadre du protocole [[OAuth 2.0 - OpenID Connect|OAuth]], la notion de client privé et public concerne la **capacité d'une application** (le client) à **stocker** de manière **sécurisée** ses **secrets** (comme les informations d'identification par exemple).

## Clients publics

Ces clients sont ceux qui ne **peuvent pas garantir** la **sécurité** de leurs informations d'identification (comme un secret client).

Cela est dû au fait qu'ils sont déployés dans des environnements où le code ou les informations peuvent être facilement examinés ou manipulés par des utilisateurs finaux ou des attaquants.
Cela inclut les **applications mobiles**, les **Single Page Applications** (SPA), et les applications exécutées sur des dispositifs avec des interfaces utilisateur limitées.

## Clients privés

Ces clients peuvent **protéger** leurs **informations** d'identification, généralement parce qu'ils sont exécutés dans des **environnements contrôlés**, comme des **serveurs sécurisés**.