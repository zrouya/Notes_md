
## Qu'est-ce que IPSec ?

IPSec est un ensemble de règles ou de protocoles de communication permettant d'établir des connexions sécurisées sur un réseau. Le protocole Internet (IP) est la norme commune qui détermine comment les données circulent sur Internet. IPSec ajoute le chiffrement et l'authentification pour rendre le protocole plus sûr. Par exemple, il chiffre les données à la source et les déchiffre à la destination. Il authentifie également la source des données. 

## En quoi IPSec est-il important ?

L'Internet Engineering Task Force a développé IPSec dans les années 1990 pour garantir la confidentialité, l'intégrité et l'authenticité des données lors de l'accès aux réseaux publics. Par exemple, les utilisateurs se connectent à Internet avec un [réseau privé virtuel (VPN)](https://aws.amazon.com/what-is/vpn/) IPSec pour accéder aux fichiers de l'entreprise à distance. Le protocole IPSec chiffre les informations sensibles pour éviter les contrôles non désirés. Le serveur peut également vérifier que les paquets de données reçus sont autorisés.

## Quelles sont les utilisations d'IPSec ?

IPsec peut être utilisé pour les fonctions suivantes :

- Assurer la sécurité du routeur lors de l'envoi de données sur l'Internet public.
- Chiffrer les données des applications.
- Authentifier rapidement les données si elles proviennent d'un expéditeur connu.
- Protéger les données du réseau en mettant en place des circuits chiffrés, appelés tunnels IPsec, qui chiffrent toutes les données envoyées entre deux points de terminaison.

Les organisations utilisent IPSec pour se protéger contre les attaques par relecture. Une attaque par relecture, ou attaque de type man-in-the-middle, consiste à intercepter et à modifier une transmission en cours en acheminant les données vers un ordinateur intermédiaire. Le protocole IPSec attribue un numéro séquentiel à chaque paquet de données et effectue des contrôles pour détecter les signes de paquets en double. 

## Qu'est-ce que le chiffrement IPSec ?

Le chiffrement IPSec est une fonction logicielle qui brouille les données afin de protéger leur contenu contre les parties non autorisées. Les données sont chiffrées par une clé de chiffrement, et une clé de déchiffrement est nécessaire pour déchiffrer les informations. IPSec prend en charge différents types de chiffrement, notamment AES, Blowfish, Triple DES, ChaCha et DES-CBC. 

IPSec utilise le chiffrement asymétrique et symétrique pour assurer la rapidité et la sécurité du transfert des données. Dans le cas du chiffrement asymétrique, la clé de chiffrement est rendue publique tandis que la clé de déchiffrement reste privée. Le chiffrement symétrique utilise la même clé publique pour le chiffrement et le déchiffrement des données. IPSec établit une connexion sécurisée avec un chiffrement asymétrique et passe au chiffrement symétrique pour accélérer le transfert des données.

## Comment fonctionne IPSec ?

Les ordinateurs échangent les données avec le protocole IPSec en suivant les étapes ci-dessous. 

1. L'ordinateur expéditeur détermine si la transmission de données nécessite une protection IPSec en effectuant une vérification par rapport à sa politique de sécurité. Si c'est le cas, l'ordinateur lance une transmission IPSec sécurisée avec l'ordinateur destinataire.
2. Les deux ordinateurs négocient les conditions requises pour établir une connexion sécurisée. Cette démarche inclut un accord mutuel sur les paramètres de chiffrement et d'authentification et d'autres associations de sécurité (AS). 
3. L'ordinateur envoie et reçoit les données chiffrées, validant qu'elles proviennent de sources fiables. Il effectue des vérifications pour s'assurer que le contenu sous-jacent est fiable. 
4. Une fois que la transmission est terminée ou que la session a expiré, l'ordinateur met fin à la connexion IPSec. 

## Que sont les protocoles IPSec ?

Les protocoles IPSec envoient les paquets de données en toute sécurité. Un paquet de données est une structure spécifique qui formate et prépare les informations pour la transmission sur le réseau. Il se compose de l'en-tête, de la charge utile et de la bande de fin.

- L'en-tête est une section précédente qui contient des informations d'instructions pour acheminer le paquet de données vers la bonne destination. 
- La charge utile est un terme qui décrit les informations réelles contenues dans un paquet de données.
- La bande de fin est une donnée supplémentaire ajoutée à la queue de la charge utile pour indiquer la fin du paquet de données. 

 Certains protocoles IPSec sont présentés ci-dessous.

### **En-tête d'authentification (AH)**

Le protocole d'en-tête d'authentification (AH) ajoute un en-tête qui contient les données d'authentification de l'expéditeur et protège le contenu du paquet contre toute modification par des parties non autorisées. Il avertit le destinataire d'éventuelles manipulations du paquet de données d'origine. Lorsqu'il reçoit le paquet de données, l'ordinateur compare le calcul du hachage cryptographique de la charge utile avec l'en-tête pour s'assurer que les deux valeurs correspondent. Un hachage cryptographique est une fonction mathématique qui résume les données en une valeur unique. 

### **Encapsulation de la charge utile de sécurité (ESP)**

Selon le mode IPSec sélectionné, le protocole ESP effectue le chiffrement de l'ensemble du paquet IP ou uniquement de la charge utile. ESP ajoute un en-tête et une bande de fin au paquet de données lors du chiffrement. 

### **IKE (Internet Key Exchange)**

L'Internet Key Exchange (IKE) est un protocole qui établit une connexion sécurisée entre deux appareils sur Internet. Les deux appareils établissent une association de sécurité (SA), qui implique la négociation de clés et d'algorithmes de chiffrement pour transmettre et recevoir les paquets de données suivants. 

## Quels sont les modes IPSec ?

IPSec fonctionne selon deux modes différents avec des degrés de protection divers. 

**Tunnel**

Le mode tunnel IPSec est adapté au transfert des données sur les réseaux publics, car il renforce la protection des données contre les parties non autorisées. L'ordinateur chiffre toutes les données, notamment la charge utile et l'en-tête, et y ajoute un nouvel en-tête. 

**Transport**

Le mode de transport IPSec chiffre uniquement la charge utile du paquet de données et laisse l'en-tête IP sous sa forme d'origine. L'en-tête de paquet non chiffré permet aux routeurs d'identifier l'adresse de destination de chaque paquet de données. Par conséquent, le transport IPSec est utilisé dans un réseau étroit et de confiance, tel que la sécurisation d'une connexion directe entre deux ordinateurs. 

## Qu'est-ce qu'un VPN IPSec ?

Le VPN (réseau privé virtuel) est un logiciel de réseaux qui permet aux utilisateurs de naviguer sur Internet de manière anonyme et sécurisée. Un VPN IPSec est un logiciel VPN qui utilise le protocole IPSec pour créer des tunnels chiffrés sur Internet. Il fournit un chiffrement de bout en bout, ce qui signifie que les données sont brouillées sur l'ordinateur et décodées sur le serveur de réception. 

### **VPN SSL** 

SSL est l'abréviation de Secure Socket Layer. Il s'agit d'un protocole de sécurité qui protège le trafic web. Un VPN SSL est un service de sécurité réseau basé sur un navigateur qui utilise le protocole SSL intégré pour chiffrer et protéger les communications réseau. 

### **Quelle est la différence entre un VPN IPSec et un VPN SSL ?**

Les deux protocoles de sécurité fonctionnent sur différentes couches du modèle d'interconnexion des systèmes ouverts (OSI). Le modèle OSI définit la structure en couches de la façon dont les ordinateurs échangent les données sur un réseau. 

Les protocoles IPSec s'appliquent aux couches réseau et transport au cœur du modèle OSI. Quant au SSL, il chiffre les données au niveau de la couche d'applications la plus élevée. Vous pouvez vous connecter à un VPN SSL à partir d'un navigateur web, par contre vous devez installer un logiciel distinct pour utiliser les VPN IPSec.